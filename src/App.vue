<script setup lang="ts">
import { ref } from 'vue';

// Referências de estado
const socket = ref<WebSocket | null>(null);
const clientId = ref<string | null>(null);

// Função para iniciar a comunicação via WebSocket
const initiateWebSocket = () => {
  // URL do servidor WebSocket
  socket.value = new WebSocket('ws://sket-soket-cvy6ep-8713dd-167-88-33-37.traefik.me');  

  socket.value.onopen = () => {
    console.log('WebSocket Connected');
  };

  // Quando receber uma mensagem do servidor
  socket.value.onmessage = (event) => {
    const data = JSON.parse(event.data as string);

    // Verifica se a mensagem contém o evento 'connected'
    if (data.event === 'connected') {
      clientId.value = data.clientId;
      console.log('Cliente registrado com ID:', clientId.value);
    }

    // Lidar com outros eventos aqui, como offer, answer, ice-candidate, etc.
    if (data.event) {
      console.log('Evento recebido:', data.event, data);
    }
  };

  // Lidar com erros
  socket.value.onerror = (error) => {
    console.error('WebSocket Error:', error);
  };

  // Quando a conexão for fechada
  socket.value.onclose = () => {
    console.log('WebSocket Connection Closed');
  };
};

// Enviar mensagem para outro cliente (oferta, resposta, candidato ICE)
const sendMessage = (event: string, target: string, message: any) => {
  if (socket.value && clientId.value) {
    const data = {
      event: event,
      target: target,
      sender: clientId.value,
      ...message,
    };
    socket.value.send(JSON.stringify(data));
    console.log(`Mensagem enviada: ${event}`, data);
  }
};
</script>

<template>
  <div class="container">
    <!-- Botão para Conectar ao WebSocket -->
    <button @click="initiateWebSocket" class="start-button">
      Conectar ao WebSocket
    </button>

    <!-- Exibir o ID do cliente -->
    <div v-if="clientId">
      <p>Seu ID: {{ clientId }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
  padding: 20px;
}

.start-button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.start-button:hover {
  background-color: #358c68;
}
</style>
