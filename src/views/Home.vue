<template>
  <div class="home">
    <h1>New Product</h1>
    <div>Name: <input type="text" v-model="newProductParams.name" /></div>
    <div>Price: <input type="text" v-model="newProductParams.price" /></div>
    <div>
      Description: <input type="text" v-model="newProductParams.description" />
    </div>
    <div>
      Image_url: <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create</button>
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>Name: {{ product.name }}</h2>
      <img :src="product.image_url" alt="" />
      <p>Price: {{ product.price }}</p>
      <p>Description: {{ product.description }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
// import func from "vue-editor-bridge";
export default {
  data: function () {
    return {
      products: [],
      newProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct: function () {
      var params = {
        name: "Water Bottle",
        price: "10",
        description: "Store your preferred H20 in this hand wash only product!",
        image_url: "",
      };

      axios.post("http://localhost:3000/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>
