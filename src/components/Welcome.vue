<template>
    <div class="welcome-container">
      <h1 class="welcome-title">Bem-vindo ao nosso Chat Global!</h1>
      <p class="welcome-text">
        Este site foi feito para ajudar pessoas a se conectarem umas com as outras ao redor do mundo. Aqui você pode conversar com outros usuários e fazer novas amizades, independentemente da distância.
      </p>
      <button @click="enterChat" class="enter-button">Entrar no Chat</button>
    </div>
  </template>
  
  <script setup>
  const emit = defineEmits(['enterChat']);
  
  const enterChat = () => {
    emit('enterChat');
  };
  </script>
  
  <style scoped>
  .welcome-container {
    text-align: center;
    padding: 50px;
    background-color: #f0f8ff;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    max-width: 500px;
    margin: auto;
  }
  
  .welcome-title {
    font-size: 36px;
    color: #333;
    margin-bottom: 20px;
  }
  
  .welcome-text {
    font-size: 18px;
    color: #555;
    margin-bottom: 30px;
    line-height: 1.6;
  }
  
  .enter-button {
    padding: 15px 30px;
    cursor: pointer;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 18px;
    transition: background-color 0.3s ease, transform 0.3s ease;
  }
  
  .enter-button:hover {
    background-color: #0056b3;
    transform: scale(1.05);
  }
  
  .enter-button:active {
    background-color: #003d80;
  }
  </style>
  