<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <GIMS msg="GIMS - Generic Inventory Management System"/>
  </div>
</template>

<script>
import GIMS from './components/GIMS.vue'
export default {
  name: 'App',
  components: {
    GIMS
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
