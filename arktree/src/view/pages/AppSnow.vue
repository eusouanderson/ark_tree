<template>
  <div ref="container" class="particle-container"></div>
</template>

<script>
import * as THREE from 'three';

export default {
  name: 'AppSnow',
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
      const maxVisibleParticles = 5000;
      this.scene = new THREE.Scene();
      this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      this.renderer = new THREE.WebGLRenderer({ alpha: true });
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(this.renderer.domElement);

      // Geometria e material para flocos de neve
      const snowflakeGeometry = new THREE.CircleGeometry(0.5, 6);
      const snowflakeMaterial = new THREE.MeshBasicMaterial({
        color: 0xFFFFFF,
        transparent: true,
        opacity: 0.8,
        blending: THREE.AdditiveBlending
      });

      this.snowflakes = [];
      for (let i = 0; i < maxVisibleParticles; i++) {
        const snowflake = new THREE.Mesh(snowflakeGeometry, snowflakeMaterial);
        this.resetSnowflakePosition(snowflake);
        this.snowflakes.push(snowflake);
        this.scene.add(snowflake);
      }

      this.camera.position.z = 5;
      this.animateSnowflakes();
    },

    resetSnowflakePosition(snowflake) {
      snowflake.position.set(
        Math.random() * window.innerWidth - window.innerWidth / 2,
        Math.random() * window.innerHeight - window.innerHeight / 2,
        Math.random() * 150 - 50
      );
      snowflake.velocity = new THREE.Vector3(0, Math.random() * 0.2 + 0.1, 0);
    },

    animateSnowflakes() {
      const animate = () => {
        this.snowflakes.forEach(snowflake => {
          snowflake.velocity.y -= 0.01;
          snowflake.position.x += (Math.random() - 0.5) * 0.1;
          snowflake.position.y += snowflake.velocity.y;

          if (snowflake.position.y < -window.innerHeight / 2) {
            this.resetSnowflakePosition(snowflake);
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

      this.snowflakes.forEach(snowflake => this.resetSnowflakePosition(snowflake));

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
  background-image: url('@/assets/images/snow-background.webp'); /* Imagem de fundo com neve */
  background-size: cover;
  background-position: center;
  transition: background-position 0.2s ease-out;
  z-index: 1;
}
</style>
