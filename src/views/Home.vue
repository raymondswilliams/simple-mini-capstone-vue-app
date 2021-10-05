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
    <p>newProductParams: {{ newProductParams }}</p>
    <button v-on:click="createProduct()">Create</button>
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>Name: {{ product.name }}</h2>
      <img :src="product.image_url" alt="" />
      <p>Price: {{ product.price }}</p>
      <p>Description: {{ product.description }}</p>
      <button v-on:click="showProduct(product)">See Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Details</h1>
        currentProduct: {{ currentProduct }}
        <p>Name: <input type="text" v-model="currentProduct.name" />/p></p>
        <p>Price: <input type="text" v-model="currentProduct.price" /></p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        Image Url: <input type="text" v-model="currentProduct.image_url" />
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import func from "vue-editor-bridge";
// import func from "vue-editor-bridge";
export default {
  data: function () {
    return {
      products: [],
      newProductParams: {},
      currentProduct: [],
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
      axios
        .post("http://localhost:3000/products", this.newProductarams)
        // happy status code/path
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        })
        // sad status code/path
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      var editProductParams = product;
      axios
        .patch(
          `http://localhost:3000/products/${product.id}`,
          editProductParams
        )
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    destroyProduct: function (product) {
      axios
        .delete(`http://localhost:3000/products/${product.id}`)
        .then((response) => {
          console.log("Killed it,", response.data);
        });
    },
  },
};
</script>
