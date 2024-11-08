<template>
  <div ref="container" class="particle-container"></div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'AppBackground',
  mounted() {
    this.initScene();
    window.addEventListener('mousemove', this.onMouseMove, { passive: true });
    window.addEventListener('touchmove', this.onMouseMove, { passive: true });
    window.addEventListener('resize', this.handleResize);
  },
  unmounted() {
    window.removeEventListener('mousemove', this.onMouseMove);
    window.removeEventListener('touchmove', this.onMouseMove);
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    initScene() {
      const maxVisibleParticles = window.innerWidth < 600 ? 500 : 1000;
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(this.renderer.domElement);

      // Reutilizando a geometria para todas as partículas
      const particleGeometry = new THREE.CircleGeometry(0.1, 5);
      const particleMaterial = new THREE.MeshBasicMaterial({
        color: 0x008000,
        transparent: true,
        opacity: 0.8,
        blending: THREE.AdditiveBlending
      });

      this.particles = [];
      for (let i = 0; i < maxVisibleParticles; i++) {
        const particle = new THREE.Mesh(particleGeometry, particleMaterial);
        this.resetParticlePosition(particle);
        this.particles.push(particle);
        this.scene.add(particle);
      }

      this.camera.position.z = 5;
      this.animateParticles();
    },

    resetParticlePosition(particle) {
      particle.position.set(
        Math.random() * window.innerWidth - window.innerWidth / 2,
        Math.random() * window.innerHeight - window.innerHeight / 2,
        Math.random() * 150 - 50
      );
      particle.velocity = new THREE.Vector3(0.1, 0.1, 0.5);
    },

    animateParticles() {
      const animate = () => {
        this.particles.forEach(particle => {
          particle.velocity.y -= 0.01;
          particle.position.x += (Math.random() - 0.5) * 0.1;
          particle.position.y += particle.velocity.y;

          if (particle.position.y < -window.innerHeight / 2) {
            this.resetParticlePosition(particle);
          }
        });

        this.renderer.render(this.scene, this.camera);
        requestAnimationFrame(animate);
      };
      animate();
    },

    onMouseMove(event) {
      const clientX = event.clientX || (event.touches && event.touches[0].clientX);
      const clientY = event.clientY || (event.touches && event.touches[0].clientY);
      const mouseX = (clientX / window.innerWidth) * 500 - 9;
      const mouseY = (clientY / window.innerHeight) * 250 + 1;

      this.updateBackgroundPosition(mouseX, mouseY);
    },

    updateBackgroundPosition(mouseX, mouseY) {
      const container = this.$refs.container;
      if (container) {
        container.style.backgroundPosition = `${50 + mouseX * 0.10}% ${50 + mouseY * 0.1}%`;
      }
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
</style>
