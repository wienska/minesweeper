<template>
  <div id="app">
    <minesweeper msg="Minesweeper"></minesweeper>
  </div>
</template>

<script>
import minesweeper from './components/minesweeper.vue'

export default {
  name: 'app',
  components: {
    minesweeper
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
