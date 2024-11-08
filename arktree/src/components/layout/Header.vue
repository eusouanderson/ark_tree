<template>
  <header class="fixed top-0 left-0 w-full p-4 bg-black bg-opacity-50 shadow-lg backdrop-blur-md z-50">
    <div class="max-w-7xl mx-auto flex justify-between items-center">
      <h1 class="text-white text-2xl font-bold">ArkTree</h1>
      <nav>
        <ul class="flex space-x-6">
          <li><a href="#home" class="text-white text-lg hover:text-yellow-400">Home</a></li>
          <li><a href="#about" class="text-white text-lg hover:text-yellow-400">Sobre</a></li>
          <li><a href="#services" class="text-white text-lg hover:text-yellow-400">Serviços</a></li>
          <li><a href="#contact" class="text-white text-lg hover:text-yellow-400">Contato</a></li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',  // Certifique-se de que o nome do componente seja único
};
</script>

<style scoped>
/* Estilo do Header fixo */
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 50;
  background-color: rgba(0, 0, 0, 0.7); /* Adiciona fundo semitransparente */
  backdrop-filter: blur(10px); /* Efeito de desfoque */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3); /* Sombra suave */
  padding: 1rem;
}
</style>
