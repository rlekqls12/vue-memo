<template>
  <div id="app">
    <MainPage />
  </div>
</template>

<script>
import MainPage from '@/pages/MainPage.vue'

export default {
  name: 'App',
  components: {
    MainPage
  }
}
</script>

<style>
  * {
    margin: 0;
  }
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    height: 100vh;
  }
</style>
