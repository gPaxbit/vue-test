<template>
  <div id="app">
    <Pane></Pane>
  </div>
</template>

<script>
import Pane from './components/Pane.vue'

export default {
  name: 'app',
  components: {
    Pane
  }
}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
    border-collapse: collapse;
    margin: 0;
    padding: 0;
  }
  body {
    overflow-x: hidden;
    display: inline-block;
    max-width: 100%;
  }
</style>
