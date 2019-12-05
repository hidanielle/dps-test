<template>
  <div id="app">
    <router-view/>
    <footer-about/>
  </div>
</template>

<script>
import FooterAbout from './components/FooterAbout.vue'

export default {
  name: 'App',
  components: {
    FooterAbout
  }
}
</script>

<style>
</style>
