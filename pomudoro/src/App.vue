<template>
  <div id="app">
    <homeComponent />
  </div>
</template>

<script>
import homeComponent from './components/Home.vue'

export default{
  name: "App",
  components: {
    homeComponent
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Inter' , sans-serif;
  color: #f85959;
}
#app {
 background: #fff8ee;
}
</style>
