<script setup>
function logout() {
  localStorage.removeItem("token")
}
</script>

<template>
  <nav>
    <router-link to="/">Products</router-link> |
    <router-link to="/dogs">Dogs</router-link> |
    <router-link to="/about">About</router-link>

    <div class="buttons">
      <router-link to="/profile"><input type="button" class="btn btn-outline-primary" value="My profile" style="margin-right: 5px"></router-link>
      <router-link to="/register"><input type="button" class="btn btn-primary btn-sm" value="Register" style="margin-right: 2px"></router-link>
      <router-link to="/login"><input type="button" class="btn btn-primary btn-sm" value="Login"></router-link>
      <router-link to="/"><input type="button" class="btn btn-secondary btn-sm" v-on:click="logout" value="Logout" style="margin-left: 2px"></router-link>
    </div>
  </nav>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
  text-align: left;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.buttons {
  float: right;
}

</style>
