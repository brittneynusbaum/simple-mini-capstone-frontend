<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: {},
      currentProduct: {}
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
        name: this.newProductParams.name,
        description: this.newProductParams.description,
        price: this.newProductParams.price,
        image_url: this.newProductParams.image_url
      }
      axios.post('http://localhost:3000/products.json', newProductParams).then(response => {
        console.log(response.data)
      })
    },
    showProduct: function (product) {
      console.log(product);

      console.log('showing product');
    }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <input v-model="newProductParams.name" />
    <input v-model="newProductParams.description" />
    <input v-model="newProductParams.price" />
    <input v-model="newProductParams.image_url" />
    <button v-on:click="createProduct()">Add product</button>

    <div v-for="product in products" v-bind:key="product.id">
      {{ product.name }}
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
  </div>
</template>

<style></style>