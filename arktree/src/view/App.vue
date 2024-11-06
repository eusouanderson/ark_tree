<template>
  <div class="welcome">
        <AppWelcome></AppWelcome>
  </div>
  <AppBackground></AppBackground>

  
</template>

<script>

import AppBackground from '../components/AppBackground.vue';
import AppWelcome from '../components/AppWelcome.vue';

export default {
  name: 'App',
  components: {
    AppBackground,
    AppWelcome
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
</style>
