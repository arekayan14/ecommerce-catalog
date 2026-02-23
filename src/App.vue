<template>
  <div id="app">
    <ProductDisplay />
  </div>
</template>

<script>
// Import komponen dari folder components
import ProductDisplay from './components/ProductDisplay.vue';

export default {
  name: 'App',
  components: {
    // mendaftarkan komponen agar bisa digunakan di template
    ProductDisplay
  }
}
</script>

<style>
/* Reset dasar agar tampilan rapi di semua browser */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>