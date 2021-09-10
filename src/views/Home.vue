<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Search for Concerts by City and Genre!</h2>
    <br />
    <h3>Search by City:</h3>
    search:
    <input type="text" v-model="citySearch" placeholder="enter city.." />
    <h3>Search by Genre:</h3>
    search:
    <select v-model="genreSearch">
      <option value="">--Select Genre--</option>
      <option value="KnvZfZ7vAvv">Alternative</option>
      <option value="KnvZfZ7vAve">Ballads/Romantic</option>
      <option value="KnvZfZ7vAvd">Blues</option>
      <option value="KnvZfZ7vAvk">Children's Music</option>
      <option value="KnvZfZ7vAeJ">Classical</option>
      <option value="KnvZfZ7vAv6">Country</option>
      <option value="KnvZfZ7vAvF">Dance/Electronic</option>
      <option value="KnvZfZ7vAva">Folk</option>
      <option value="KnvZfZ7vAv1">Hip-Hop/Rap</option>
      <option value="KnvZfZ7vAvJ">Holiday</option>
      <option value="KnvZfZ7vAvE">Jazz</option>
      <option value="KnvZfZ7vAJ6">Latin</option>
      <option value="KnvZfZ7vAvt">Metal</option>
      <option value="KnvZfZ7vAvn">New Age</option>
      <option value="KnvZfZ7vAvl">Other</option>
      <option value="KnvZfZ7vAev">Pop</option>
      <option value="KnvZfZ7vAee">R&B</option>
      <option value="KnvZfZ7vAed">Reggae</option>
      <option value="KnvZfZ7vAe7">Religious</option>
      <option value="KnvZfZ7vAeA">Rock</option>
      <option value="KnvZfZ7vAe6">Undefined</option>
    </select>
    <br />
    <br />
    <button @click="indexEvents">Search</button>
    <br />
    <br />
    <br />
    <hr />
    <h1>Events</h1>
    <hr />
    <ul>
      <li v-for="error in errors" :key="error">{{ error }}</li>
    </ul>
    <div v-for="event in events" :key="event.id">
      <h3>{{ event.name }}</h3>
      <img v-bind:src="event.images[1].url" v-bind:alt="event.name" />
      <h4>City:</h4>
      <p>{{ event._embedded.venues[0].city.name }}, {{ event._embedded.venues[0].state.name }}</p>
      <h4>Venue:</h4>
      <p>{{ event._embedded.venues[0].name }}</p>
      <h4>Date:</h4>
      <p>{{ event.dates.start.localDate }}</p>
      <button v-on:click="showTicket(event)">Event / Ticket Info</button>
      <dialog type="button" id="ticket-details">
        <form method="dialog">
          <h3>{{ currentTicket.name }}</h3>
          <img v-bind:src="currentTicket.images[1].url" v-bind:alt="event.name" />
          <h4>Venue:</h4>
          <p>{{ currentTicket._embedded.venues[0].name }}</p>
          <h4>City:</h4>
          <p>{{ currentTicket._embedded.venues[0].city.name }}, {{ currentTicket._embedded.venues[0].state.name }}</p>
          <h4>Adress:</h4>
          <p>{{ currentTicket._embedded.venues[0].address.line1 }}</p>
          <h4>Date:</h4>
          <p>{{ currentTicket.dates.start.localDate }}</p>
          <h4>Time:</h4>
          <p>{{ currentTicket.dates.start.localTime }}</p>
          <h4>Sub Genre</h4>
          <p>{{ currentTicket.classifications[0].subGenre.name }}</p>
          <h4>Sales:</h4>
          <p>Start: {{ currentTicket.sales.public.startDateTime }}</p>
          <p>End: {{ currentTicket.sales.public.endDateTime }}</p>
          <h4>Tickets:</h4>
          <p>
            <a :href="currentTicket.url">{{ currentTicket.url }}</a>
          </p>
          <button>Close</button>
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
      message: "Welcome to Sound Link!",
      errors: [],
      events: [],
      citySearch: "",
      genreSearch: "",
      currentTicket: {},
    };
  },
  methods: {
    indexEvents: function () {
      this.errors = [];
      let apiKey = process.env.VUE_APP_TICKETMASTER_TOKEN;
      axios
        .get(
          `https://app.ticketmaster.com/discovery/v2/events.json?size=10&city=${this.citySearch}&genreId=${this.genreSearch}&classificationId=KZFzniwnSyZfZ7v7nJ&apikey=${apiKey}`
        )
        .then((response) => {
          console.log("events index", response);
          this.events = response.data._embedded.events;
        })
        .catch((error) => {
          this.events = [];
          console.log(error.response);
          this.errors = ["No Search Results Available.."];
          this.citySearch = "";
          this.genreSearch = "";
        });
    },
    showTicket: function (event) {
      this.currentTicket = event;
      console.log(this.currentTicket);
      document.querySelector("#ticket-details").showModal();
    },
  },
};
</script>
