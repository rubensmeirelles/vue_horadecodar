<template>
  <Navbar />
  <router-view />
  <Footer />
</template>

<script>
import Navbar from './components/navbar.vue';
import Footer from './components/Footer.vue';

export default {
  components: {
    Navbar,
    Footer
  }
}
</script>

<style scoped>

</style>
