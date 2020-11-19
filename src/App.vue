<template>
  <div id="app">
      <Game />
  </div>
</template>

<script>
import Game from './components/Game.vue';

export default {
  name: 'App',
  components: {
    Game,
  },
};
</script>

<style lang="scss">
  body {
    margin: 0;
    padding: 0;
    background-color: #f5f5f5;
    width: 100%;
    height: 100%;
  }
</style>
