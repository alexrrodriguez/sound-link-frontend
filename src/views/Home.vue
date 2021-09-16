<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr />
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
      <h4>Tickets:</h4>
      <button>
        <a :href="event.url">Buy Tickets!</a>
      </button>

      <dialog type="button" id="ticket-details">
        <form method="dialog">
          <h3>{{ currentTicket.name }}</h3>
          <img v-bind:src="currentImage" />
          <h4>Venue:</h4>
          <p>{{ currentVenue.name }}</p>
          <h4>City:</h4>
          <p>{{ currentCity }}, {{ currentStateCode }}</p>
          <h4>Adress:</h4>
          <p>{{ currentAddress }}</p>
          <h4>Date:</h4>
          <p>{{ currentStart }}</p>
          <h4>Time:</h4>
          <p>{{ currentTime }}</p>
          <h4>Tickets:</h4>
          <p>
            <button><a :href="currentTicket.url">Buy Tickets!</a></button>
          </p>
          <hr />
          <button @click="addConcert">Add Concert To Your Schedule!</button>
          <br />
          <br />
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
      message: "Welcome to Sound Link!",
      errors: [],
      events: [],
      currentTicket: {},
      currentVenue: [],
      currentImage: [],
      currentStart: {},
      currentTime: {},
      currentSubGenre: {},
      currentCity: {},
      currentStateCode: {},
      currentAddress: {},
      currentPromoter: {},
      citySearch: "",
      genreSearch: "",
      newUserConcert: {},
    };
  },
  methods: {
    indexEvents: function () {
      this.errors = [];
      let apiKey = process.env.VUE_APP_TICKETMASTER_TOKEN;

      axios
        .get(
          `discovery/v2/events.json?size=10&city=${this.citySearch}&genreId=${this.genreSearch}&classificationId=KZFzniwnSyZfZ7v7nJ&apikey=${apiKey}`
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
      this.currentImage = this.currentTicket.images[0].url;
      this.currentVenue = this.currentTicket._embedded.venues[0];
      this.currentStart = this.currentTicket.dates.start.localDate;
      this.currentTime = this.currentTicket.dates.start.localTime;
      this.currentCity = this.currentTicket._embedded.venues[0].city.name;
      this.currentStateCode = this.currentTicket._embedded.venues[0].state.stateCode;
      this.currentAddress = this.currentTicket._embedded.venues[0].address.line1;
      console.log("current ticket", this.currentTicket);
      document.querySelector("#ticket-details").showModal();
    },
    addConcert: function () {
      this.newUserConcert.name = this.currentTicket.name;
      this.newUserConcert.venue = this.currentVenue.name;
      this.newUserConcert.city = this.currentCity;
      this.newUserConcert.address = this.currentAddress;
      this.newUserConcert.date = this.currentStart;
      this.newUserConcert.time = this.currentTime;
      this.newUserConcert.tickets = this.currentTicket.url;
      this.newUserConcert.image = this.currentImage;
      console.log(this.newUserConcert.name);
      console.log(this.newUserConcert.venue);
      console.log(this.newUserConcert.city);
      console.log(this.newUserConcert.address);
      console.log(this.newUserConcert.date);
      console.log(this.newUserConcert.time);
      console.log(this.newUserConcert.tickets);
      axios
        .post("http://localhost:3000/user_concerts", this.newUserConcert)
        .then((response) => {
          console.log("user concert created", response);
          this.$router.push("/profile");
        })
        .catch((error) => {
          console.log("user concert create errror", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
