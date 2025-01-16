<template>
  <div class="chat-container">
    <div v-if="messages.length" class="messages">
      <h2>Mensagens Recebidas:</h2>
      <div v-for="(msg, index) in visibleMessages" :key="index" class="message">
        <strong>{{ msg.from }}:</strong> {{ msg.message }}
      </div>
    </div>

    <textarea v-model="messageInput" placeholder="Digite sua mensagem..." rows="4"></textarea>
    <button @click="handleSendMessage" :disabled="!messageInput.trim()">Enviar Mensagem</button>

    <p><strong>Seu ID de cliente:</strong> {{ clientId }}</p>
    <p><strong>Par conectado:</strong> {{ partnerId }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  clientId: String,
  partnerId: String,
  messages: Array
});

const emit = defineEmits(['sendMessage']);

const messageInput = ref('');

// Computed property para exibir até 5 mensagens
const visibleMessages = computed(() => {
  return props.messages.slice(-5); // Exibe as últimas 5 mensagens
});

const handleSendMessage = () => {
  if (messageInput.value.trim()) {
    emit('sendMessage', messageInput.value);
    messageInput.value = '';
  }
};
</script>

<style scoped>
.chat-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #fafafa;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  color: #333;
  font-size: 24px;
  margin-bottom: 20px;
}

.messages {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
  max-height: 300px;
  overflow-y: auto;
  margin-bottom: 20px;
}

.message {
  padding: 10px;
  margin-bottom: 12px;
  background-color: #e9ecef;
  border-radius: 6px;
  transition: background-color 0.3s ease;
}

.message:hover {
  background-color: #d0d9e1;
}

.message strong {
  font-weight: 600;
}

textarea {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  resize: none;
  font-size: 16px;
  color: #333;
}

textarea:focus {
  border-color: #007BFF;
  outline: none;
}

button {
  width: 100%;
  padding: 12px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

p {
  font-size: 16px;
  color: #666;
  text-align: center;
  margin-top: 20px;
}

p strong {
  color: #333;
}
</style>
