<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: [
        {

        }
      ]
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      console.log('loading products');
      axios.get('http://localhost:3000/products.json').then(response => {
        console.log(response.data);
        this.products = response.data
      });
    },
    createProduct: function () {
      console.log('creating product');
      var newProductParams = {
        name: "Pen",
        description: "Good for writing",
        price: 15,
        image_url: "https://m.media-amazon.com/images/I/711F6-eLS6L._AC_SS450_.jpg"
      }
      axios.post('http://localhost:3000/products.json', newProductParams).then(response => {
        console.log(response.data)
      })
    }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <input v-model /> -->
    <button v-on:click="createProduct()">Add product</button>
    <div v-for="product in products" v-bind:key="product.id">{{ product.name }}</div>
  </div>
</template>

<style></style>