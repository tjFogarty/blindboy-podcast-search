<template>
  <div id="app">
    <PodcastList />
  </div>
</template>

<script>
import 'normalize.css'
import PodcastList from './components/PodcastList.vue'

export default {
  name: 'app',
  components: {
    PodcastList
  }
}
</script>

<style>
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  
  #app {
    max-width: 1000px;
    margin: 40px auto;
    padding: 0 15px;
  }
</style>
