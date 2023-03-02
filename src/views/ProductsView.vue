<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [],
      page: 0
    }
  },

  methods: {
    async goPageForward() {
      if ((await axios.get("/api/public/products4?page=" + (this.page + 1) + "&orderBy=id")).data.length > 0) {
        this.page++
        this.products = (await axios.get("/api/public/products4?page=" + this.page + "&orderBy=id")).data
        console.log(this.products)
      }
    },

    async goPageBack() {
      if (this.page > 0) {
        this.page--
        this.products = (await axios.get("/api/public/products4?page=" + this.page + "&orderBy=id")).data
        console.log(this.products)
      }
    },

    async viewProduct(productId) {
      localStorage.setItem("productId", productId)
    }
  },

  async created() {
    this.products = (await axios.get("/api/public/products4?page=0&orderBy=id")).data;
  },
}
</script>

<template>
  <div>
    <h4>Page: {{ this.page + 1 }}</h4>
  </div>
  <div class="col-sm-6 mx-auto" style="padding: 5px">
    <input type="button" v-on:click="goPageBack" class="btn btn-primary btn-sm" style="margin-right: 5px" value="←">
    <input type="button" v-on:click="goPageForward" class="btn btn-primary btn-sm" style="margin-left: 5px" value="→">
  </div>

  <div class="col-sm-10 mx-auto">
    <table>
      <caption></caption>
      <tr>
        <th></th>
        <th></th>
        <th></th>
      </tr>
      <tr v-for="product of products" :key="product.id">
        <td>
          <router-link to="/product/buy">
            <div class="hover10">
              <div class="container">
                <figure>
                  <img v-bind:src="'/api/public/images/' + product.imageId" class="rounded-card" alt="image" v-on:click="viewProduct(product.id)">
                  <div class="text-centered">VIEW PRODUCT</div>
                </figure>
              </div>
            </div>
          </router-link>
        </td>
        <td>
          <ul class="no-bullets">
            <li><strong>{{ product.name }}</strong></li>
            <li>{{ product.price }} €</li>
          </ul>
        </td>
        <td>{{ product.description }}</td>
      </tr>
    </table>
  </div>
</template>

<style scoped>

table {
}

td {
  width: 33%;
  border-bottom: 1px solid #ababab;
}

th {
  padding: 10px;
  border-bottom: 1px solid #ababab;
}

img {
  max-height: 500px;
  max-width: 500px;
  height: auto;
  width: auto;
  padding: 20px;
}

.rounded-card{
  border-radius: 15%;
}

.hover10 figure img {
  -webkit-filter: blur(0);
  filter: blur(0);
  -webkit-transition: .3s ease-in-out;
  transition: .3s ease-in-out;
}

.hover10 figure:hover img {
  -webkit-filter: blur(3px);
  filter: blur(3px);
}

.container {
  position: relative;
  text-align: center;
  color: white;
}

.container:hover .text-centered {
  opacity: 1;
}

.text-centered {
  opacity: 0;
  transition: .5s ease;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 35px;
}

ul.no-bullets {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  margin-bottom: 10px;
}
</style>