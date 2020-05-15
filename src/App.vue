<template>
  <div id="app">
    <window-print></window-print>
  </div>
</template>

<script>
import WindowPrint from "./components/WindowPrint";

export default {
  name: 'app',
  components: {
    WindowPrint
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
  margin-top: 50px;
}
</style>
