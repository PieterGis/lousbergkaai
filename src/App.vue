<template>
  <div id="app">
    <ApartmentListing />
  </div>
</template>

<script>
import ApartmentListing from './components/ApartmentListing.vue'

export default {
  name: 'App',
  components: {
    ApartmentListing
  }
}
</script>

<style>
@import '@fortawesome/fontawesome-free/css/all.min.css';
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';

@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

#app {
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
