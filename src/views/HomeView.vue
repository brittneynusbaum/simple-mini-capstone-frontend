<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductParams: {
        name: "",
        description: "",
        price: "",
        image_url: ""
      },
      currentProduct: {},
      editProductParams: {}
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
      };
      axios.post('http://localhost:3000/products.json', newProductParams).then(response => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductParams = {}
      })
    },
    showProduct: function (product) {
      console.log(product);
      console.log('showing product');
      this.currentProduct = product;
      this.editProductParams = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function () {
      console.log('updating product');
      console.log(this.editProductParams);
      axios.patch(`http://localhost:3000/products/${this.editProductParams.id}.json`, this.editProductParams).then(response => {
        console.log(response.data);

      });
    },
    // destroyProduct: function (theProduct) {
    //   console.log('destroying product');
    //   axios.delete(`http://localhost:3000/products`).then(response => {
    //     console.log(response.data);
    //   });
    // }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      Name:
      <input v-model="newProductParams.name" />
    </p>
    <p>
      Description:
      <input v-model="newProductParams.description" />
    </p>
    <small v-if="newProductParams.description.length > 0">{{ newProductParams.description.length }} character
      count</small>
    <p>
      Price:
      <input v-model="newProductParams.price" />
    </p>
    <p>
      Image Url:
      <input v-model="newProductParams.image_url" />
    </p>
    <button v-on:click="createProduct()">Add product</button>
    <div v-for="product in products" v-bind:key="product.id">
      {{ product.name }}
    </div>
    <button v-on:click="showProduct(product)">More Info</button>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Image: {{ currentProduct.image_url }}</p>
        <button>Close</button>
      </form>
      <p>
        Name:
        <input v-model="editProductParams.name" />
      </p>
      <p>
        Description:
        <input v-model="editProductParams.description" />
      </p>
      <p>
        Price:
        <input v-model="editProductParams.price" />
      </p>
      <p>
        Image Url:
        <input v-model="editProductParams.image_url" />
      </p>
      <button v-on:click="updateProduct()">Update Product</button>
      <button v-on:click="destroyProduct(currentProduct)">Delete Product</button>
    </dialog>
  </div>
</template>

<style>
</style>