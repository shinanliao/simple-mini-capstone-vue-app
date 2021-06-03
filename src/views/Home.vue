<template>
  <div class="home">
    <h2>My Products</h2>
    Title:
    <input type="text" v-model="newProductTitle" />
    <br />
    Description:
    <input type="text" v-model="newProductDescription" />
    <br />
    Price:
    <input type="text" v-model="newProductPrice" />
    <br />
    Image Url:
    <input type="text" v-model="newProductImageUrl" />
    <br />
    <button v-on:click="createProduct()">Create Product</button>

    <div v-for="product in products" v-bind:key="product.id">
      <h3>Title: {{ product.name }}</h3>
      <img :src="product.image_url" alt="" /><br />
      <button v-on:click="showProduct(product)">More Info</button>
      <p>Description: {{ product.description }}</p>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <img src="" alt="" />
        <p>Title: ...</p>
        <p>Description: ...</p>
        <p>Price: ...</p>
        <p>ImageUrl: ...</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
      newProductTitle: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
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
        name: this.newProductTitle,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.unshift(response.data);
          this.newProductTitle = "";
          this.newProductDescription = "";
          this.newProductPrice = "";
          this.newProductImageUrl = "";
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    showProduct: function (product) {
      console.log(product);
      document.querySelector("#product-details").showModal();
    }
  },
};
</script>
