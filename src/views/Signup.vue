<template>
  <div class="signup">
    <!-- Loading screen -->
    <div class="loading">
      <svg width="65px" height="65px" viewBox="0 0 66 66" xmlns="http://www.w3.org/2000/svg">
        <g>
          <animateTransform
            attributeName="transform"
            type="rotate"
            values="0 33 33;270 33 33"
            begin="0s"
            dur="1.4s"
            fill="freeze"
            repeatCount="indefinite"
          />
          <circle
            fill="none"
            stroke-width="6"
            stroke-linecap="round"
            cx="33"
            cy="33"
            r="30"
            stroke-dasharray="187"
            stroke-dashoffset="610"
          >
            <animate
              attributeName="stroke"
              values="#28c3c6;#5224a7;#fff;#5224a7;#28c3c6"
              begin="0s"
              dur="5.6s"
              fill="freeze"
              repeatCount="indefinite"
            />
            <animateTransform
              attributeName="transform"
              type="rotate"
              values="0 33 33;135 33 33;450 33 33"
              begin="0s"
              dur="1.4s"
              fill="freeze"
              repeatCount="indefinite"
            />
            <animate
              attributeName="stroke-dashoffset"
              values="187;46.75;187"
              begin="0s"
              dur="1.4s"
              fill="freeze"
              repeatCount="indefinite"
            />
          </circle>
        </g>
      </svg>
    </div>
    <!-- Loaded content -->
    <div class="loaded">
      <section class="section-bg filter-section events">
        <div class="container">
          <div class="row home-search-header">
            <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
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
              <form v-on:submit.prevent="submit()">
                <h1>Username:</h1>
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
                <small
                  v-if="newUserParams.password.length > 0 && newUserParams.password.length < 6"
                  class="text-danger"
                >
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
      console.log(this.newUserParams);
      axios
        .post("/users", this.newUserParams)
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
