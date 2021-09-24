<template>
  <div class="signup">
    <section class="section-bg filter-section events">
      <div class="container">
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <form v-on:submit.prevent="submit()">
              <h2>Signup</h2>
              <h5>
                Or
                <a href="/login">Login</a>
                If Already Registered!
              </h5>
              <br />
              <ul>
                <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
              </ul>
              <h1>Name:</h1>
              <div>
                <input class="solid-input form artist-input" type="text" v-model="newUserParams.name" />
              </div>
              <small>{{ 50 - newUserParams.name.length }} characters remaining</small>
              <h1>Email:</h1>
              <div>
                <input class="solid-input form artist-input" type="email" v-model="newUserParams.email" />
              </div>
              <h1>Image URL:</h1>
              <div>
                <input class="solid-input form artist-input" type="text" v-model="newUserParams.image" />
              </div>
              <h1>Password:</h1>
              <div>
                <input class="solid-input form artist-input" type="password" v-model="newUserParams.password" />
              </div>
              <small v-if="newUserParams.password.length > 0 && newUserParams.password.length < 6" class="text-danger">
                Password must be 6 characters
              </small>
              <small v-if="newUserParams.password.length > 20" class="text-danger">
                Password cannot exceed 20 characters
              </small>
              <h1>Password confirmation:</h1>
              <div>
                <input
                  class="solid-input form artist-input"
                  type="password"
                  v-model="newUserParams.password_confirmation"
                />
              </div>
              <small v-if="newUserParams.password_confirmation !== newUserParams.password" class="text-danger">
                Must match Password!
                <br />
              </small>
              <br />
              <input class="btn btn-primary ripple" type="submit" value="Submit" />
            </form>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: { name: "", password: "", password_confirmation: "" },
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("http://localhost:3000/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
