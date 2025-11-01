<script>
import ProductDisplay from './components/ProductDisplay.vue';

export default {
  name: 'App',
  components: {
    ProductDisplay
  }
};
</script>

<template>
  <ProductDisplay />
</template>

<style>
@import './assets/style/page.css';
body {
    font-family: sans-serif;
    margin: 0;
}
</style>