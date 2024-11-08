<template>
  <div>
    <!-- Spinner de Carregamento -->
    <div v-if="isLoading" class="spinner">
      <div class="loader"></div>
      <p class="loading-text"> loading...</p>
    </div>

    <!-- Seus Componentes -->
    <div v-else>
      <div v-if="showWelcome" class="welcome">
        <AppWelcome></AppWelcome>
      </div>

      <div class="description" ref="description" v-if="!showWelcome">
        <AppDescription></AppDescription>
      </div>
      
      <!-- Pagina Florest -->
      <AppFlorest v-if="currentPage === 1"></AppFlorest>
      <!-- Pagina Snow -->
      <AppSnow v-if="currentPage === 2"></AppSnow>

      <!-- Botões de Navegação -->
      <div class="nav-buttons">
        <button
          v-if="currentPage === 2"
          @click="goToFirstPage"
          class="nav-button back-button"
        >
          <span class="arrow">← </span>Back
        </button>
        <button
          v-if="currentPage === 1"
          @click="goToNextPage"
          class="nav-button next-button"
        >
          Next <span class="arrow">→</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AppWelcome from '../components/text/Welcome.vue';
import AppDescription from '../components/text/Description.vue';
import AppSnow from './pages/AppSnow.vue';
import AppFlorest from './pages/AppFlorest.vue';

export default {
  name: 'App',
  components: {
    AppFlorest,
    AppWelcome,
    AppDescription,
    AppSnow
  },
  data() {
    return {
      isLoading: true, // Variável de controle do spinner
      showWelcome: true, // Controle de exibição do AppWelcome
      currentPage: 1, // Página atual (1 = Florest, 2 = Snow)
    };
  },
  mounted() {
    setTimeout(() => {
      this.isLoading = false; // Esconde o spinner após o tempo de carregamento
    }, 2000);

    // Adiciona o evento de scroll
    window.addEventListener("wheel", this.handleScroll);
  },
  beforeUnmount() {
    // Remove o evento de scroll ao destruir o componente
    window.removeEventListener("wheel", this.handleScroll);
  },
  methods: {
    goToNextPage() {
      this.currentPage = 2; // Avança para a página Snow
    },
    goToFirstPage() {
      this.currentPage = 1; // Volta para a página Florest
    },
    handleScroll(event) {
      // Detecta a direção do scroll
      if (event.deltaY > 0 && this.showWelcome) {
        // Rolou para baixo, navega para a descrição e desativa o AppWelcome
        this.scrollToDescription();
        this.showWelcome = false;
      } else if (event.deltaY < 0 && !this.showWelcome) {
        // Rolou para cima, retorna para o AppWelcome
        this.scrollToWelcome();
        this.showWelcome = true;
      }
    },
    scrollToDescription() {
      this.$nextTick(() => {
        const descriptionElement = this.$refs.description;
        if (descriptionElement) {
          descriptionElement.scrollIntoView({ behavior: 'smooth' });
        }
      });
    },
    scrollToWelcome() {
      this.$nextTick(() => {
        const welcomeElement = this.$el.querySelector('.welcome');
        if (welcomeElement) {
          welcomeElement.scrollIntoView({ behavior: 'smooth' });
        }
      });
    }
  }
};
</script>

<style scoped>
.welcome,
.description {
  color: #f3f3f3;
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
  flex-direction: column;
}

.loader {
  border: 5px solid #f3f3f3;
  border-top: 5px solid #0b1d29;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
}

.loading-text {
  margin-top: 20px;
  font-size: 24px;
  font-weight: 600;
  color: #070a0d;
  letter-spacing: 1px;
  animation: fadeIn 1.5s ease-in-out;
}

.nav-buttons {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 3; /* Assegura que os botões fiquem sobre os outros elementos */
}

.nav-button {
  z-index: 0;
  background: rgba(0, 0, 0, 0.421);
  color: #fff;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s ease;
}

.nav-button:hover {
  background: rgba(0, 0, 0, 0.8);
}

.arrow {
  font-size: 18px;
  margin-left: 10px;
  margin-right: 10px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
