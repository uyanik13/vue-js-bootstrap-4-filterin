<template>
  <div id="app">
    <case />
  </div>
</template>

<script>
import Case from './components/Case.vue'

export default {
  name: 'App',
  components: {
    Case
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
