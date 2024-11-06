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
    window.addEventListener('mousemove', this.onMouseMove);
    window.addEventListener('resize', this.handleResize);
  },
  unmounted() {
    window.removeEventListener('mousemove', this.onMouseMove);
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    initScene() {
      const maxVisibleParticles = 10000; // Limite máximo de partículas visíveis
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(this.renderer.domElement);

      this.particles = [];
      this.createParticles(maxVisibleParticles);

      this.camera.position.z = 5;

      this.animateParticles();
    },

    createParticles(maxVisibleParticles) {
      for (let i = 0; i < maxVisibleParticles; i++) {
        const particleGeometry = new THREE.CircleGeometry(0.1, 5); // a geometria diretamente aqui
        const particleMaterial = new THREE.MeshBasicMaterial({
          color: 0x008000,
          transparent: true,
          opacity: 0.7
        }); // Criando o material diretamente aqui

        const particle = new THREE.Mesh(particleGeometry, particleMaterial);
        this.resetParticlePosition(particle);
        this.particles.push(particle);
        this.scene.add(particle);
      }
    },

    resetParticlePosition(particle) {
      particle.position.x = Math.random() * window.innerWidth - window.innerWidth / 2;
      particle.position.y = Math.random() * window.innerHeight - window.innerHeight / 2;
      particle.position.z = Math.random() * 150 - 50;
    },

    animateParticles() {
      const animate = () => {
        for (let i = 0; i < this.particles.length; i++) {
          this.particles[i].position.y -= 0.10;

          if (this.particles[i].position.y < -window.innerHeight / 2) {
            this.particles[i].position.y = window.innerHeight / 2;
          }
        }

        this.renderer.render(this.scene, this.camera);
        requestAnimationFrame(animate);
      };
      animate();
    },

    onMouseMove(event) {
      const mouseX = (event.clientX / window.innerWidth) * 500 - 9;
      const mouseY = (event.clientY / window.innerHeight) * 250 + 1;

      // Movimentação das partículas em relação ao movimento do mouse
      for (let i = 0; i < this.particles.length; i++) {
        const dx = this.particles[i].position.x - mouseX;
        const dy = this.particles[i].position.y - mouseY;
        const dist = Math.sqrt(dx * dx + dy * dy);

        if (dist < 20) {
          const repulsionFactor = 0.1;
          this.particles[i].position.x += dx * repulsionFactor;
          this.particles[i].position.y += dy * repulsionFactor;
        }
      }

      this.updateBackgroundPosition(mouseX, mouseY);
    },

    updateBackgroundPosition(mouseX, mouseY) {
      const container = this.$refs.container;
      container.style.backgroundPosition = `${50 + mouseX * 0.10}% ${50 + mouseY * 0.1}%`;
    },

    handleResize() {
      const width = window.innerWidth;
      const height = window.innerHeight;

      this.camera.aspect = width / height;
      this.camera.updateProjectionMatrix();

      this.particles.forEach(particle => this.resetParticlePosition(particle));

      this.renderer.setSize(width, height);
    }
  }
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
  background-image: url('@/assets/images/background.webp');
  background-size: cover;
  background-position: center;
  transition: background-position 0.2s ease-out;
  z-index: 1;
}

.overlay-gif {
  position: absolute;
  top: 0;
  left: 0;
  width: 10%;
  height: 150%;
  background-image: url('@/assets/images/borboletas.gif');
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0.7;
  pointer-events: none;
  z-index: 2;
}
</style>
