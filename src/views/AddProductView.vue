<template>
  <div class="row">
    <div class="col-sm-3 mx-auto">
      <h1>Add new product</h1>
      <br>
      <form class="row g-3">
        <div class="col-12">
          <label for="inputProductName" class="form-label">Name</label>
          <input type="text" class="form-control" id="inputProductName" v-model="posts.name" placeholder="e.g. Milk">
        </div>
        <div class="col-12">
          <div class="form-floating">
            <label for="inputProductDescription" class="form-label">Description</label>
            <textarea class="form-control" v-model="posts.description" placeholder="e.g. Very tasty, I milked it myself..." id="inputProductDescription" style="height: 100px"></textarea>
          </div>
        </div>
        <div class="col-md-6">
          <label for="inputProductPrice" class="form-label">Price (€)</label>
          <input type="text" pattern="[0-9.]+[0-9]{2}" class="form-control" id="inputProductPrice" v-model="posts.price" placeholder="0.00">
        </div>
        <div class="col-md-6">
          <label for="inputProductCategory" class="form-label">Category</label>
          <input list="categories" v-model="posts.categoryName" placeholder="Choose..." required id="inputProductCategory">
          <datalist id="categories">
            <option value="Animals"></option>
            <option value="Meal"></option>
            <option value="Furniture"></option>
            <option value="Electronics"></option>
            <option value="Clothes"></option>
          </datalist>
          <br>
          <br>
        </div>
        <div class="col-md-12">
          <label for="inputImage" class="form-label">Image</label>
          <input type="file" ref="uploadImage" @change="onImageUpload()" class="form-control" required id="inputImage">
          <input type="button" @click="startUpload" name="Upload" value="Upload">
        </div>
        <div class="col-12">
          <router-link to="/success"><input type="button" v-on:click="postData" class="btn btn-primary" value="Add"></router-link>
        </div>
      </form>
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
        userId: null,
        name: null,
        price: null,
        description: null,
        categoryId: null,
        categoryName: null,
        imageId: null,
      },
      formData: null,
      uploaded: false
    }
  },
  methods: {
    postData() {
      let token = JSON.parse(localStorage.getItem("token"))
      if (token != null) {

        if (this.uploaded) {
          let userData = VueJwtDecode.decode(token);
          this.posts.userId = userData["id"]
          this.posts = axios.post('/api/public/products', this.posts)
        } else {
          alert("Upload product image")
          router.back()
        }
      } else {
        alert("User not logged in.")
        router.back()
      }
    },

    onImageUpload() {
      let file = this.$refs.uploadImage.files[0];
      this.formData = new FormData();
      this.formData.append("file", file);
    },

    startUpload() {
      axios({
        url: "/api/public/images",
        method: "POST",
        data: this.formData,
        headers: {
          Accept: "application/json",
          "Content-Type": "multipart/form-data"
        },
      })
      this.uploaded = true;
    }
  }
};
</script>

<style scoped>
label {
  display: block;
  font: 1rem 'Fira Sans', sans-serif;
}

input,
label {
  margin: 0.4rem 0;
}
</style>