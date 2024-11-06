<template>
  <div ref="container" class="particle-container">
    <!-- Adicionando a camada do GIF aqui -->
    
  </div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'AppBackground',
  mounted() {
    this.initScene();
  },
  methods: {
    initScene() {
      const maxVisibleParticles = 10000; // Limite máximo de partículas visíveis
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      const renderer = new THREE.WebGLRenderer({ alpha: true });
      renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(renderer.domElement);

      const particles = [];
      const particleGeometry = new THREE.CircleGeometry(0.1, 3);
      const particleMaterial = new THREE.MeshBasicMaterial({
        color: 0x008000,
        transparent: true,
        opacity: 0.7
      });

      for (let i = 0; i < maxVisibleParticles; i++) {
        const particle = new THREE.Mesh(particleGeometry, particleMaterial);
        particle.position.x = Math.random() * window.innerWidth - window.innerWidth / 2;
        particle.position.y = Math.random() * window.innerHeight - window.innerHeight / 2;
        particle.position.z = Math.random() * 150 - 50;
        particles.push(particle);
        scene.add(particle);
      }

      camera.position.z = 5;

      const animateParticles = () => {
        for (let i = 0; i < particles.length; i++) {
          particles[i].position.y -= 0.10;

          if (particles[i].position.y < -window.innerHeight / 2) {
            particles[i].position.y = window.innerHeight / 2;
          }
        }

        renderer.render(scene, camera);
        requestAnimationFrame(animateParticles);
      };
      animateParticles();

      const onMouseMove = (event) => {
        const mouseX = (event.clientX / window.innerWidth) * 500 - 9;
        const mouseY = (event.clientY / window.innerHeight) * 250 + 1;

        // Aplica uma leve repulsão nas partículas quando estão muito próximas do cursor
        for (let i = 0; i < particles.length; i++) {
          const dx = particles[i].position.x - mouseX;
          const dy = particles[i].position.y - mouseY;
          const dist = Math.sqrt(dx * dx + dy * dy);

          if (dist < 20) {  // Aumente ou diminua esse valor para ajustar o efeito
            const repulsionFactor = 0.1; // Força da repulsão
            particles[i].position.x += dx * repulsionFactor;
            particles[i].position.y += dy * repulsionFactor;
          }
        }

        // Atualiza a posição do fundo, mas sem afetar o GIF
        this.updateBackgroundPosition(mouseX, mouseY);
      };

      window.addEventListener('mousemove', onMouseMove, false);
    },

    updateBackgroundPosition(mouseX, mouseY) {
      const container = this.$refs.container;
      // Movimenta o fundo conforme o movimento do mouse, mas sem afetar o GIF
      container.style.backgroundPosition = `${50 + mouseX * 0.10}% ${50 + mouseY * 0.1}%`;
    },
  },
};
</script>

<style scoped>
.particle-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background-image: url('@/assets/images/background.webp'); /* Imagem de fundo */
  background-size: 2048px 1080px;
  background-position: center;
  transition: background-position 0.2s ease-out;
  z-index: 1; /* Camada inferior */
}

.overlay-gif {
  position: absolute;
  top: 0;
  left: 0;
  width: 10%;
  height: 150%;
  background-image: url('@/assets/images/borboletas.gif'); /* GIF animado */
  background-size: contain; /* Ajuste para manter as proporções do GIF */
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0.7; /* Transparência ajustável para o GIF */
  pointer-events: none; /* Permite clicar nos elementos abaixo do GIF */
  z-index: 2; /* Camada superior ao fundo */
}
</style>
