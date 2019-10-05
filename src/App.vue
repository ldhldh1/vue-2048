<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <header2048></header2048>
    <game-board></game-board>  
  </div>
</template>

<script>
import Header2048 from "./components/Header2048.vue";
import GameBoard from "./components/GameBoard.vue"

export default {
  name: "app",
  components: {
    Header2048,
    GameBoard
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
