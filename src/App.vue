<template>
  <div id="app">
    <Widget msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Widget from './components/Widget.vue'

export default {
  name: 'app',
  components: {
    Widget
  }
}
</script>

<style>
#app {
  /*font-family: 'Avenir', Helvetica, Arial, sans-serif;*/
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
