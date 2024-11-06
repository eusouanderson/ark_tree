<template>
  <div>
    <!-- Spinner de Carregamento -->
    <div v-if="isLoading" class="spinner">
      <div class="loader"></div>
      <p class="loading-text"> Carregando...</p>
    </div>

    <!-- Seus Componentes -->
    <div v-else>
      <div class="welcome">
        <AppWelcome></AppWelcome>
      </div>
      <AppBackground></AppBackground>
    </div>
  </div>
</template>

<script>
import AppBackground from '../components/AppBackground.vue';
import AppWelcome from '../components/AppWelcome.vue';

export default {
  name: 'App',
  components: {
    AppBackground,
    AppWelcome
  },
  data() {
    return {
      isLoading: true // Variável de controle do spinner
    };
  },
  mounted() {
    // Simulação de um tempo de carregamento ou substitua por um evento real
    setTimeout(() => {
      this.isLoading = false; // Esconde o spinner após o tempo de carregamento
    }, 2000); // 2 segundos para o exemplo, ajuste conforme necessário
  }
}
</script>

<style scoped>
.welcome {
  color: white;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  z-index: 2;
}

.spinner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column; /* Alinha o texto abaixo do spinner */
}

.loader {
  border: 5px solid #f3f3f3;
  border-top: 5px solid #0b1d29;
  border-radius: 50%;
  width: 30px; /* Aumenta o tamanho do spinner */
  height: 30px; /* Aumenta o tamanho do spinner */
  animation: spin 1s linear infinite; /* Acelera a animação */
}

.loading-text {
  margin-top: 20px;
  font-size: 24px; /* Aumenta o tamanho da fonte */
  font-weight: 600; /* Torna o texto mais ousado */
  color: #070a0d; /* Cor elegante para o texto */
  letter-spacing: 1px; /* Espaçamento entre as letras */
  animation: fadeIn 1.5s ease-in-out; /* Animação suave para o texto */
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@keyframes fadeIn {
  0% { opacity: 0; }
  100% { opacity: 1; }
}
</style>
