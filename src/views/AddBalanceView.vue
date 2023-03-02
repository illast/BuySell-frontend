<template>
  <div class="row">
    <div class="col-sm-3 mx-auto">
      <h1>Add balance</h1>
      <span class="text-black-50">Current balance: {{this.posts.balance}} EUR</span>
      <br>
      <form class="row g-3">
        <div class="col-12">
          <br>
          <label for="inputUserBalance" class="form-label">Amount (€)</label>
          <input type="text" class="form-control" id="inputProductName" v-model="addBalance" placeholder="0.00">
          <br>
        </div>
        <div class="col-12">
          <router-link to="/success">
            <input type="button" v-on:click="postData" class="btn btn-primary" value="Add">
          </router-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data(){
    return {
      posts: {
        balance: null,
      },
      addBalance: null,
    }
  },
  methods: {
    postData() {
      this.posts.balance = parseFloat(this.posts.balance) + parseFloat(this.addBalance)
      this.posts = axios.put('/api/users/' + localStorage.getItem("userId"), this.posts)
      localStorage.setItem("userBalance", this.posts.balance)
    },
  },

  created() {
    this.posts.balance = localStorage.getItem("userBalance");
  }
};
</script>

<style scoped>
</style>