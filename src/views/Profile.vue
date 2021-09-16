<template>
  <div class="profile-page">
    <div>
      <h1>{{ user.name }}</h1>
      <p>{{ user.email }}</p>
      <img :src="user.image" :alt="user.name" />
    </div>
    <h1>{{ user.name }} Concert Schedule:</h1>
    <hr />
    <div v-for="concert in concerts" :key="concert.id">
      <h2>{{ concert.name }}</h2>
      <img v-bind:src="concert.image" v-bind:alt="concert.name" />
      <h4>{{ concert.venue }}</h4>
      <p>{{ concert.date }}</p>
      <p>{{ concert.time }}</p>
      <button v-on:click="showTicket(concert)">Event / Ticket Info</button>
      <dialog type="button" id="ticket-details">
        <form method="dialog">
          <h2>{{ currentTicket.name }}</h2>
          <img v-bind:src="currentTicket.image" />
          <h4>{{ currentTicket.venue }}</h4>
          <h4>{{ currentTicket.city }}</h4>
          <p>{{ currentTicket.address }}</p>
          <p>{{ currentTicket.date }}</p>
          <p>{{ currentTicket.time }}</p>
          <p>
            <button><a :href="currentTicket.tickets">Ticket URL</a></button>
          </p>
          <button>Back</button>
        </form>
      </dialog>
      <hr />
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
      currentTicket: {},
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
    showTicket: function (concert) {
      this.currentTicket = concert;
      console.log("current ticket", this.currentTicket);
      document.querySelector("#ticket-details").showModal();
    },
  },
};
</script>
