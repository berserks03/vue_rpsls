<template>
  <div id="app">
    <h1>Rock, Paper, Scissors, Lizard, Spock.</h1>
    <Game />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Game from './components/game/Game.vue';

export default defineComponent({
  name: 'App',
  components: {
    Game,
  },
});
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
body {
  background-color: #f0eeee;
}
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  user-select: none;
  -moz-user-select: none;
  -khtml-user-select: none;
  -webkit-user-select: none;
  -o-user-select: none;
}

img {
  -webkit-user-drag: none;
  -khtml-user-drag: none;
  -moz-user-drag: none;
  -o-user-drag: none;
}
</style>
