<template>
  <div class="login">
    <section class="section-bg filter-section events">
      <div class="container">
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <h2>Login</h2>
            <br />
            <form v-on:submit.prevent="submit()">
              <ul>
                <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
              </ul>
              <h1>Email:</h1>
              <div>
                <input
                  placeholder="enter email.."
                  class="solid-input form artist-input"
                  type="email"
                  v-model="newSessionParams.email"
                />
              </div>
              <br />
              <h1>Password:</h1>
              <div>
                <input
                  placeholder="enter password.."
                  class="solid-input form artist-input"
                  type="password"
                  v-model="newSessionParams.password"
                />
              </div>
              <br />
              <input class="btn btn-primary ripple" type="submit" value="Submit" />
            </form>
            <br />
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
      newSessionParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("http://localhost:3000/sessions", this.newSessionParams)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>
