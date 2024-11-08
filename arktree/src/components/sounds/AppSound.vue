<template>
  <div class="ambient-sound">
    <button @click="toggleSound" class="play-pause-btn">
      {{ isPlaying ? 'Pause' : 'Play' }}
    </button>
    <div class="volume-control">
      <label for="volume">Volume</label>
      <input type="range" id="volume" min="0" max="1" step="0.1" v-model="volume" @input="setVolume" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'AmbientSound',
  data() {
    return {
      audio: new Audio(require('@/assets/sounds/ambient-sound.mp3')), // Substitua pelo caminho do seu arquivo de som
      isPlaying: false,
      volume: 0.5, // Volume inicial
    };
  },
  methods: {
    toggleSound() {
      if (this.isPlaying) {
        this.audio.pause();
      } else {
        this.audio.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    setVolume() {
      this.audio.volume = this.volume;
    },
  },
  watch: {
    volume(newVolume) {
      this.audio.volume = newVolume;
    },
  },
};
</script>

<style scoped>
.ambient-sound {
  text-align: center;
  padding: 10px;
}

.play-pause-btn {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  cursor: pointer;
  margin-bottom: 10px;
}

.play-pause-btn:hover {
  background-color: #45a049;
}

.volume-control {
  display: flex;
  align-items: center;
  justify-content: center;
}

input[type="range"] {
  width: 100px;
  margin-left: 10px;
}
</style>
