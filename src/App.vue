<script>
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    Weather
  }
}
</script>


<template>
  <div id="app">
    <Weather />
  </div>
</template>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {  
  background-image: url(./assets/cold-bg.jpg);
  background-size: cover;
  background-position: botton;
  transition: .4s;
}
</style>
