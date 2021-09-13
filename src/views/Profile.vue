<template>
  <div class="profile-page">
    <div>
      <h1>{{ user.name }}</h1>
      <p>{{ user.email }}</p>
      <img :src="user.image" :alt="user.name" />
    </div>
    <div>
      <h1>Attending:</h1>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      user: [],
    };
  },
  created: function () {
    this.indexUser();
  },
  methods: {
    indexUser: function () {
      axios.get("http://localhost:3000/users").then((response) => {
        console.log("user index", response);
        this.user = response.data;
      });
    },
  },
};
</script>
