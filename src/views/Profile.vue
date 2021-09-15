<template>
  <div class="profile-page">
    <div>
      <h1>{{ user.name }}</h1>
      <p>{{ user.email }}</p>
      <img :src="user.image" :alt="user.name" />
    </div>
    <h1>{{ user.name }} Concert Schedule:</h1>
    <div v-for="concert in concerts" :key="concert.id">
      <h1>{{ concert.name }}</h1>
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
      concerts: [],
    };
  },
  created: function () {
    this.indexUser();
    this.indexUserConcerts();
  },
  methods: {
    indexUser: function () {
      axios.get("http://localhost:3000/users").then((response) => {
        console.log("user index", response);
        this.user = response.data;
      });
    },
    indexUserConcerts: function () {
      axios.get("http://localhost:3000/user_concerts").then((response) => {
        console.log("user concerts index", response);
        this.concerts = response.data;
      });
    },
  },
};
</script>
