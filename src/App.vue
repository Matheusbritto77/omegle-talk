<template>
  <div id="app">
    <h1>Chat entre Clientes</h1>

    <!-- Tela inicial -->
    <ChatWaiting v-if="currentPhase === 'waiting'" />
    
    <!-- Tela do chat -->
    <ChatRoom v-else-if="currentPhase === 'chat'" :clientId="clientId" :partnerId="partnerId" :messages="messages" @sendMessage="sendMessage" />
    
    <!-- Tela inicial -->
    <Welcome v-if="currentPhase === 'start'" @enterChat="enterChat" />
  </div>
</template>

<script setup>
import { ref, onBeforeUnmount } from 'vue';
import ChatWaiting from './components/ChatWaiting.vue';
import ChatRoom from './components/ChatRoom.vue';
import Welcome from './components/Welcome.vue';

const currentPhase = ref('start');
const clientId = ref(null);
const partnerId = ref(null);
const messages = ref([]);
let socket = null;

const enterChat = () => {
  currentPhase.value = 'waiting';
  connectToWebSocket();
};

const connectToWebSocket = () => {
  socket = new WebSocket('ws://sket-soket-cvy6ep-8713dd-167-88-33-37.traefik.me');

  socket.onopen = () => {
    console.log('Conectado ao servidor WebSocket');
  };

  socket.onmessage = async (event) => {
    try {
      const decodedData = await decodeMessage(event.data);

      if (decodedData && decodedData.event) {
        if (decodedData.event === 'connected') {
          if (!clientId.value) {
            clientId.value = decodedData.clientId;
          } else {
            partnerId.value = decodedData.clientId;
            currentPhase.value = 'chat';
          }
        } else if (decodedData.event === 'message') {
          messages.value.push({
            from: decodedData.from || 'Desconhecido',
            message: decodedData.message
          });
        } else if (decodedData.event === 'disconnected') {
          alert('O parceiro desconectou.');
          currentPhase.value = 'waiting';
          partnerId.value = null;
        }
      }
    } catch (error) {
      console.error('Erro ao processar mensagem:', error);
    }
  };

  socket.onclose = () => {
    console.log('Desconectado do servidor WebSocket');
    currentPhase.value = 'start';
    clientId.value = null;
    partnerId.value = null;
  };
};

const sendMessage = (message) => {
  if (message && socket && socket.readyState === WebSocket.OPEN) {
    socket.send(
      JSON.stringify({
        event: 'message',
        message: message,
      })
    );
    messages.value.push({ from: 'Você', message });
  }
};

const decodeMessage = async (data) => {
  // Verifica se os dados são um ArrayBuffer
  if (data instanceof ArrayBuffer) {
    const buffer = new Uint8Array(data);
    
    // Decodifica o buffer para uma string usando TextDecoder
    const decodedMessage = new TextDecoder('utf-8', { fatal: true }).decode(buffer);
    
    try {
      // Tenta converter a string decodificada para JSON
      return JSON.parse(decodedMessage);
    } catch (e) {
      console.warn('Falha ao parsear a mensagem decodificada:', e);
      return decodedMessage; // Retorna o texto decodificado se não for JSON válido
    }
  }

  // Verifica se os dados são uma string
  if (typeof data === 'string') {
    try {
      // Tenta converter a string para JSON
      return JSON.parse(data);
    } catch (e) {
      console.warn('Falha ao parsear a mensagem string:', e);
      return data; // Se falhar, retorna a string como está
    }
  }

  // Caso os dados não sejam nem ArrayBuffer nem string, retorna os dados brutos
  return data;
};

onBeforeUnmount(() => {
  if (socket) {
    socket.close();
  }
});
</script>
