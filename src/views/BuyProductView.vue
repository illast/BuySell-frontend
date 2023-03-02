<script>
import axios from "axios";
import router from "@/router";

export default {
  data(){
    return {
      users: [],
      product: [],
      userSeller: [],
      productImage: null,

      trade: {
        buyerId: null,
        sellerId: null,
        date: null
      },

      posts: {
        balance: null,
      },
    }
  },

  methods: {
    async buyProduct() {

      let userBalance = parseFloat(this.posts.balance)
      let productPrice = parseFloat(this.product.price)

      if (userBalance - productPrice >= 0) {

        this.trade.buyerId = parseInt(localStorage.getItem("userId"));
        this.trade.sellerId = this.product.userId;
        this.trade.date = new Date();

        await axios.post('/api/trades/' + this.product.id, this.trade)

        this.posts.balance = userBalance - productPrice
        await axios.put('/api/users/' + localStorage.getItem("userId"), this.posts)
        localStorage.setItem("userBalance", this.posts.balance)

        this.userSeller = (await axios.get('/api/users/' + this.trade.sellerId)).data
        this.posts.balance = parseFloat(this.userSeller.balance) + productPrice
        await axios.put('/api/users/' + this.trade.sellerId, this.posts)
        this.link = "/success"
      }

      else {
        alert("Not enough money")
        router.back()
      }
    }
  },

  async created() {
    let productId = localStorage.getItem("productId");
    let response = await axios.get('/api/public/product/' + productId);
    this.product = response.data;
    this.posts.balance = localStorage.getItem("userBalance");
  }
}
</script>

<template>
  <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">

  <div class="container">
    <div class="card">
      <div class="card-body">
        <h1 class="card-title">{{ this.product.name }}</h1>
        <h4 class="card-subtitle">{{ this.product.categoryName }}</h4>
        <div class="row">
          <div class="col-lg-5 col-md-5 col-sm-6">
            <div class="white-box text-center"><img v-bind:src="'/api/public/images/' + this.product.imageId" alt=""></div>
          </div>
          <div class="col-lg-7 col-md-7 col-sm-6">
            <h2 class="box-title mt-5">Product description</h2>
            <p>{{ this.product.description }}</p>
            <h2 class="mt-5">
              €{{ this.product.price }}<small class="text-success"> (36%off)</small>
            </h2>
            <router-link to="/success"><button class="btn btn-primary btn-rounded" v-on:click="buyProduct()">Buy Now</button></router-link>
            <h4 class="box-title mt-5">Key Highlights</h4>
            <ul class="list-unstyled">
              <li><em class="fa fa-check text-success"></em> MasterCard, Visa, PayPal</li>
              <li><em class="fa fa-check text-success"></em> Money back guarantee within 30 days</li>
              <li><em class="fa fa-check text-success"></em> 100% security - protection of personal data</li>
              <li><em class="fa fa-check text-success"></em> In case of a problem, call us on 620 2002</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
body{
  background-color: #edf1f5;
  margin-top:20px;
}
.card {
  margin-bottom: 30px;
}
.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 0 solid transparent;
  border-radius: 0;
}
.card .card-subtitle {
  font-weight: 300;
  margin-bottom: 10px;
  color: #8898aa;
}
.table-product.table-striped tbody tr:nth-of-type(odd) {
  background-color: #f3f8fa!important
}
.table-product td{
  border-top: 0 solid #dee2e6 !important;
  color: #728299!important;
}
img {
  width: 430px;
  margin-top: 90px;
}
</style>