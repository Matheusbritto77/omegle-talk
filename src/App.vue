<script setup lang="ts">
import { ref } from 'vue';

// Referências de estado
const loading = ref(false);
const matchFound = ref(false);
const uid = ref<string | null>(null);
const socket = ref<WebSocket | null>(null);
const inQueue = ref(false); // Controle para verificar se está na fila

// Função para gerar um UID único
const generateUID = (): string => {
  return 'xxxx-xxxx-xxxx-xxxx'.replace(/[x]/g, () => (Math.random() * 16 | 0).toString(16));
};

// Função para entrar na fila e aguardar a correspondência
const joinQueue = () => {
  loading.value = true;
  inQueue.value = true;
  uid.value = generateUID();  // Gerar o UID único para o usuário
  console.log(`User UID: ${uid.value}`);
  
  // Simulação de espera para encontrar outro usuário
  setTimeout(() => {
    loading.value = false;
    matchFound.value = true;
    initiateWebSocket();
  }, 3000);  // Tempo simulado para encontrar um usuário (3 segundos)
};

// Função para sair da fila
const leaveQueue = () => {
  loading.value = false;
  inQueue.value = false;
  matchFound.value = false;
  console.log('User left the queue');
};

// Função para iniciar a comunicação via WebSocket
const initiateWebSocket = () => {
  if (!uid.value) {
    console.error('UID não disponível.');
    return;
  }

  // Inicialize o WebSocket somente se `uid.value` estiver definido
  socket.value = new WebSocket('ws://sket-soket-cvy6ep-8713dd-167-88-33-37.traefik.me/');
  
  socket.value.onopen = () => {
    console.log(`WebSocket Connected with UID: ${uid.value}`);
    socket.value?.send(JSON.stringify({ uid: uid.value, action: 'startMeet' }));
  };

  socket.value.onmessage = (event) => {
    console.log('Message from WebSocket:', event.data);
  };

  socket.value.onerror = (error) => {
    console.error('WebSocket Error:', error);
  };

  socket.value.onclose = () => {
    console.log('WebSocket Connection Closed');
  };
};
</script>

<template>
  <div class="container">
    <!-- Botão para Iniciar Meet -->
    <button v-if="!inQueue.value" @click="joinQueue" :disabled="loading.value" class="start-button">
      Iniciar Meet
    </button>

    <!-- Botão para Sair da fila -->
    <button v-if="inQueue.value" @click="leaveQueue" class="leave-button">
      Sair da fila
    </button>

    <!-- Spinner de Carregamento -->
    <div v-if="loading.value" class="loading-spinner">
      <span class="spinner"></span> Carregando...
    </div>

    <!-- Mensagem e Imagem quando o Match for Encontrado -->
    <div v-if="matchFound.value" class="meet-ready">
      <p>Match encontrado! Conectando via WebSocket...</p>
      <img src="https://via.placeholder.com/150" alt="Vídeo chamada" class="video-call-image" />
    </div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
  padding: 20px;
}

.start-button, .leave-button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.start-button:disabled {
  background-color: #ccc;
}

.start-button:hover {
  background-color: #358c68;
}

.leave-button {
  background-color: #ff4757;
  margin-top: 10px;
}

.leave-button:hover {
  background-color: #e84118;
}

.loading-spinner {
  margin-top: 20px;
  font-size: 18px;
}

.spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #42b883;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.meet-ready {
  margin-top: 20px;
}

.video-call-image {
  width: 100px;
  height: 100px;
  margin-top: 10px;
  border-radius: 50%;
}
</style>
