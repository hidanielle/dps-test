<template>
  <div id="app" class="site-container">
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

<style lang="scss">
@import "./assets/styles/app.scss";
</style>
