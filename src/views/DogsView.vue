<template>
  <div class="row">
    <div class="col-sm-3 mx-auto">
      <h1>See dogs here</h1>
      <form @submit="loadMeme">
        <button type="submit" class="btn btn-primary">Next dog</button>
      </form>
      <br>
      <div class="row g-3">
        <img class="marginauto" :src=meme alt="">
        <h5>Dog pictures are updated every 10 minutes</h5>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data(){
    return {
      meme: ""
    }
  },

  methods: {
    loadMeme() {
      this.meme = axios.get('/api/public/meme/')
    }
  },

  async created() {
    try {
      let response = await axios.get('/api/public/meme/');
      this.meme = response.data;
    } catch (e) {
      console.error(e);
    }
  }
};
</script>

<style>
img {
  display: block;
  max-width:500px;
  max-height:500px;
  width: auto;
  height: auto;
}

.marginauto {
  margin: 10px auto 20px;
  display: block;
}
</style>