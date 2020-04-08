<template>
  <div id="app">
    <Qrcode />
  </div>
</template>

<script>
import Qrcode from './components/Qrcode.vue'

export default {
  name: 'App',
  components: {
    Qrcode
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
