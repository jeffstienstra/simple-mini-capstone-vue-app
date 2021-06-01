<template>
  <div class="home">
    <div>
      <h1>Create a New Product</h1>
      Name
      <div style="text-align: center">
        <input v-model="newProductName" placeholder="name" />
      </div>
      <br />
      Description
      <div style="text-align: center">
        <input v-model="newProductDescription" placeholder="description" />
      </div>
      <br />
      Price
      <div>
        <input v-model="newProductPrice" placeholder="price" />
      </div>
      <br />
      Image URL
      <div>
        <input v-model="newProductImageUrl" placeholder="image_url" />
      </div>
      <div>
        <br />
        <button v-on:click="createProduct()">Create</button>
      </div>
    </div>
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id" style="text-align: center">
      <h2>{{ product.name }}</h2>
      <p>Price: {{ product.price }}</p>
      <img :src="product.image_url" style="width: 40%" />
      <p>__________________________________________________________</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      products: [],
      newProductName: "",
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
        this.products = response.data;
        console.log("All Products", this.products);
      });
    },
    createProduct: function () {
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl,
        price: this.newProductPrice,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          console.log(response.data);
          this.newProductName = "";
          this.newProductDescription = "";
          this.newProductPrice = "";
          this.newProductImageUrl = "";
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
