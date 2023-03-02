<template>
  <div class="row">
    <div class="col-sm-3 mx-auto">
      <h1>Register</h1>
      <br>
      <form class="row g-3">
        <div class="col-md-6">
          <label for="inputFirstName" class="form-label">First name</label>
          <input type="text" class="form-control" id="inputFirstName" v-model="posts.firstName" placeholder="e.g. John">
        </div>
        <div class="col-md-6">
          <label for="inputLastName" class="form-label">Last name</label>
          <input type="text" class="form-control" id="inputLastName" v-model="posts.lastName" placeholder="e.g. Snow">
        </div>
        <div class="col-12">
          <label for="inputEmail" class="form-label">Email</label>
          <input type="email" class="form-control" id="inputEmail" v-model="posts.email" placeholder="eg@example.com">
        </div>
        <div class="col-12">
          <label for="inputPassword" class="form-label">Password</label>
          <input type="password" class="form-control" id="inputPassword" v-model="posts.password" placeholder="Password" style="margin-bottom: 10px">
        </div>
        <div class="col-12">
          <input type="password" class="form-control" id="inputPasswordRepeat" placeholder="Repeat password">
        </div>
        <div class="col-md-6">
          <label for="inputBirthdate" class="form-label">Birthdate</label>
          <input type="date" class="form-control" id="inputBirthdate" v-model="posts.birthdate" placeholder>
        </div>
        <div class="col-md-6">
          <label for="inputGender" class="form-label">Gender</label>
          <div class="form-group">
            <input list="genders" name="gender" id="inputGender" v-model="posts.gender" placeholder="Choose...">
            <datalist id="genders">
              <option value="Male"></option>
              <option value="Female"></option>
              <option value="Bigender"></option>
              <option value="Binary"></option>
              <option value="Cis"></option>
              <option value="Cismale"></option>
              <option value="Cisfemale"></option>
              <option value="Cisman"></option>
              <option value="Ciswoman"></option>
              <option value="Demi-guy"></option>
              <option value="Demi-girl"></option>
              <option value="Female to male"></option>
              <option value="Male to female"></option>
              <option value="FTM"></option>
              <option value="MTF"></option>
              <option value="Genderfluid"></option>
              <option value="Intergender"></option>
              <option value="Multigender"></option>
              <option value="Maverique"></option>
              <option value="Neurogender"></option>
              <option value="Non-binary"></option>
              <option value="Other"></option>
              <option value="Pangender"></option>
              <option value="Trans"></option>
              <option value="Transgender"></option>
              <option value="Trans male"></option>
              <option value="Trans female"></option>
              <option value="Trans person"></option>
              <option value="Transsexual"></option>
              <option value="Trigender"></option>
              <option value="Two spirit"></option>
              <option value="Attack helicopter"></option>
              <option value="Other"></option>
              <option value="Prefer not to specify"></option>
            </datalist>
          </div>
          <br>
        </div>
        <div class="col-12">
          <router-link to="/success"><button type="submit" class="btn btn-primary" v-on:click=postData>Create account</button></router-link>
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
        firstName: null,
        lastName: null,
        email: null,
        password: null,
        birthdate: null,
        gender: null,
        balance: 0,
      }
    }
  },
  methods: {
    postData() {
      this.posts = axios.post("/api/public/users", this.posts)
    }
  }
};
</script>

<style>
label {
  margin-top: 15px;
}
</style>