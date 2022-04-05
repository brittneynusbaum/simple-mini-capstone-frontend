<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function () {
    console.log('loading products');
    axios.get('http://localhost:3000/products').then(response => {
      console.log(response.data);
      this.products = response.data
    })
  },
  methods: {
    indexProducts: function () {

    }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products" v-bind:key="product.id">{{ product.name }}</div>
  </div>
</template>

<style></style>