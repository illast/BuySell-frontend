<template>
  <div class="login mt-4">
    <div class="row">
      <div class="col-sm-3 mx-auto">
        <h1>Update product</h1>
        <br>
        <form>
          <div class="mb-3">
            <label for="inputProductName">Name</label>
            <input type="text" class="form-control" id="inputProductName" v-model="posts.name" placeholder="New name">
          </div>
          <div class="mb-3">
            <label for="inputProductDescription">Description</label>
            <div class="form-floating">
              <textarea class="form-control" id="inputProductDescription" v-model="posts.description" placeholder="New description" style="height: 100px"></textarea>
            </div>
          </div>
          <div class="mb-3">
            <label for="inputProductPrice">Price (€)</label>
            <input type="text" class="form-control" id="inputProductPrice" v-model="posts.price" placeholder="New price (€)">
          </div>
          <div class="mb-3">
          <input list="categories" id="inputProductCategory" v-model="posts.categoryName" placeholder="Choose category" required>
            <datalist id="categories">
              <option value="Animals"></option>
              <option value="Meal"></option>
              <option value="Furniture"></option>
              <option value="Electronics"></option>
              <option value="Clothes"></option>
            </datalist>
          </div>
          <router-link to="/success"><input type="button" v-on:click="postData" class="btn btn-primary" value="Update"></router-link>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import VueJwtDecode from 'vue-jwt-decode'
import router from "@/router";

export default {
  data(){
    return {
      posts: {
        id: null,
        userId: null,
        name: localStorage.getItem("productName"),
        description: localStorage.getItem("productDescription"),
        categoryId: null,
        categoryName: null,
        price: localStorage.getItem("productPrice"),
        imageId: localStorage.getItem("imageId"),
      }
    }
  },
  methods: {
    postData() {
      let token = JSON.parse(localStorage.getItem("token"))
      if (token != null) {
        let userData = VueJwtDecode.decode(token);
        this.posts.userId = userData["id"]
        this.posts.id = localStorage.getItem("productId")
        this.posts = axios.put('/api/public/products/' + this.posts.id, this.posts)
      }
      else {
        alert("User not logged in.")
        router.back()
      }
    }
  }
};
</script>

<style scoped>

</style>