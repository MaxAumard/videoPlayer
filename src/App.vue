<template>
  <div id="app">
    <Player />
  </div>
</template>

<script>
import Player from './components/Player.vue';

export default {
  components: {
    Player,
  },
};
</script>

<style>
#app {
  background:black;
  overflow: hidden;
}
</style>
