<template>
  <div id="app">
    <h1>Red Dead Redemption on 386dx (no, just a canvas)</h1>
    <vue-pixelify :pixelSize="7" :src="require('./assets/example.png')" fillTransparencyColor="black"/>
  </div>
</template>

<script>
import VuePixelify from './components/vue-pixelify'

export default {
  name: 'app',
  components: {
    VuePixelify
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
