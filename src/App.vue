<template>
  <div id = 'app'>
    <MyGraph/>
  </div>
</template>

<script>
import MyGraph from '@/components/MyGraph'
export default {
  name: 'app',
  components: {
    MyGraph
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
