<template>
  <div class="loading-container">
    <h1 class="loading-title">Conectando...</h1>
    <div class="loading-message">
      <p><span class="emoji">🔌</span> Conectando ao servidor...</p>
      <p><span class="emoji">🤝</span> Aguardando um parceiro...</p>
    </div>
    <div class="loading-spinner"></div>
  </div>
</template>

<script setup>
</script>

<style scoped>
.loading-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 50px;
  background-color: #f0f8ff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: auto;
  text-align: center;
}

.loading-title {
  font-size: 36px;
  color: #333;
  margin-bottom: 20px;
  font-weight: bold;
}

.loading-message p {
  font-size: 20px;
  color: #555;
  margin-bottom: 15px;
  font-family: 'Arial', sans-serif;
  display: flex;
  align-items: center;
  gap: 10px;
}

.emoji {
  font-size: 28px;
}

.loading-spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #007BFF;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin-top: 20px;
}

/* Animação de rotação */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
