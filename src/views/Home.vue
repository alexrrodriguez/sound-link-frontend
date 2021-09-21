<template>
  <div class="home">
    <!-- Header content -->
    <header class="jumbotron">
      <div class="container" id="headline-slide">
        <div class="row headline no-gutters">
          <div class="col-md-7 text-right align-middle welcome-title">
            <h1 class="headline-left">
              <span class="header-welcome">Welcome to</span>

              <br />
              <span class="header-title">Sound Link</span>
            </h1>
          </div>
          <div class="col-md-5 text-left">
            <h1 class="headline-right">Discover Your Sound</h1>
          </div>
          <div class="col-md-12 text-center hidden-sm-down">
            <a class="btn btn-rounded ripple" id="discover" href="/signup">Sign up</a>
          </div>
          <!-- <div class="col-12 hidden-md-up">
            <a class="btn btn-rounded ripple" id="discover">discover</a>
          </div> -->
        </div>
      </div>
      <!-- <div class="add-event-jumbotron">
        <h4>Add event</h4>
        <button type="button" class="btn btn-fab" id="add-event">
          <img src="img/plusIcon.svg" alt="Add a new event" />
        </button>
      </div> -->
    </header>
    <!-- Item Filter section -->
    <section class="section-bg filter-section events">
      <div class="container">
        <br />
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <h2>Search For Events</h2>
          </div>

          <div class="col-lg-12 col-md-12">
            <h1 class="chart-title-header">Search by City:</h1>
            <input class="solid-input form artist-input" type="text" v-model="citySearch" placeholder="enter city.." />
            <br />
            <br />
            <h1 class="chart-title-header">Search by Genre:</h1>
            <br />
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
            <br />
            <button @click="indexEvents">Search</button>
            <hr />
            <ul>
              <li v-for="error in errors" :key="error">{{ error }}</li>
            </ul>
          </div>
        </div>

        <div class="row justify-content-center no-gutters match-height">
          <div
            class="col-12 col-sm-12 col-md-6 col-lg-4 event-card event-rap event-anniversary event-romance"
            v-for="event in events"
            :key="event.id"
          >
            <!--Card-->
            <div class="card hoverable">
              <div class="view overlay img-frame">
                <img v-bind:src="event.images[1].url" v-bind:alt="event.name" />
                <a v-on:click="showTicket(event)">
                  <div class="img-mask"></div>
                </a>
              </div>
              <!--Card content-->
              <div class="card-block">
                <!--Title-->
                <h4 class="card-title">{{ event.name }}</h4>
                <!--Text-->
                <br />
                <h5 class="card-text">{{ event._embedded.venues[0].name }}</h5>
                <h5 class="card-text">
                  {{ event._embedded.venues[0].city.name }}, {{ event._embedded.venues[0].state.name }}
                </h5>
                <div class="filter-card-details">
                  <ul>
                    <li>
                      <i class="fa fa-map-marker" aria-hidden="true">{{ event._embedded.venues[0].address.line1 }}</i>
                    </li>
                    <li>
                      <i class="fa fa-calendar" aria-hidden="true"></i>
                      {{ event.dates.start.localDate }}
                    </li>
                    <li>
                      <i class="fa fa-clock-o" aria-hidden="true"></i>
                      {{ event.dates.start.localTime }}
                    </li>
                    <hr />
                    <li>
                      <button>
                        <a :href="event.url">Buy Tickets!</a>
                      </button>
                    </li>
                  </ul>
                </div>
              </div>
              <!--/.Card content-->
            </div>
            <!--/.Card-->
          </div>
        </div>
        <hr />
        <br />
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
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <h2>Upcoming Events</h2>
          </div>
        </div>
        <!-- No Content available -->
        <div class="row grid-no-data">
          <div class="col-md-12 text-center">
            <h1 class="no-content">No content available</h1>
          </div>
        </div>

        <!-- All latest events -->
        <div class="row">
          <div class="col-md-12">
            <a class="pluss-button" href="/events.html">
              <i class="fa fa-plus" aria-hidden="true"></i>
              <h5>All latest events</h5>
            </a>
          </div>
        </div>
        <section class="section-bg filter-section events">
          <div class="container">
            <!-- Content row starts -->

            <div class="row no-gutters justify-content-center match-height">
              <div
                class="col-12 col-sm-12 col-md-6 col-lg-4 event-card event-rap event-anniversary event-romance"
                v-for="randomTicket in randomTickets"
                :key="randomTicket.id"
              >
                <!--Card-->
                <div class="card hoverable">
                  <div class="view overlay img-frame">
                    <img v-bind:src="randomTicket.images[0].url" v-bind:alt="randomTicket.name" />
                    <a v-on:click="showRandomTicket(randomTicket)">
                      <div class="img-mask"></div>
                    </a>
                  </div>
                  <!--Card content-->
                  <div class="card-block">
                    <!--Title-->
                    <h4 class="card-title">{{ randomTicket.name }}</h4>
                    <!--Text-->
                    <br />
                    <h5 class="card-text">
                      {{ randomTicket._embedded.venues[0].city.name }},
                      {{ randomTicket._embedded.venues[0].state.stateCode }}
                    </h5>
                    <h5 class="card-text">{{ randomTicket._embedded.venues[0].name }}</h5>
                    <div class="filter-card-details">
                      <ul>
                        <li>
                          <i class="fa fa-map-marker" aria-hidden="true">
                            {{ randomTicket._embedded.venues[0].address.line1 }}
                          </i>
                        </li>
                        <li>
                          <i class="fa fa-calendar" aria-hidden="true">: {{ randomTicket.dates.start.localDate }}</i>
                        </li>
                        <li>
                          <i class="fa fa-clock-o" aria-hidden="true">: {{ randomTicket.dates.start.localDate }}</i>
                        </li>
                        <hr />
                        <button>
                          <a :href="randomTicket.url">Buy Tickets!</a>
                        </button>
                      </ul>
                    </div>
                  </div>
                  <!--/.Card content-->
                </div>
                <!--/.Card-->
              </div>
            </div>
          </div>
        </section>
      </div>
      <!--/.container-->
    </section>
    <!-- <hr />
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
      <h4>Tickets:</h4>
      <button>
        <a :href="event.url">Buy Tickets!</a>
      </button>

      <hr />
    </div> -->
  </div>
</template>

<style>
.home-search-header {
  text-align: center;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
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
      randomTickets: [],
    };
  },
  created: function () {
    this.randomTicket();
  },
  methods: {
    indexEvents: function () {
      this.errors = [];
      let apiKey = process.env.VUE_APP_TICKETMASTER_TOKEN;

      axios
        .get(
          `discovery/v2/events.json?size=48&city=${this.citySearch}&genreId=${this.genreSearch}&classificationId=KZFzniwnSyZfZ7v7nJ&apikey=${apiKey}`
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
    randomTicket: function () {
      let apiKey = process.env.VUE_APP_TICKETMASTER_TOKEN;
      axios
        .get(`discovery/v2/events.json?size=12&sort=random&countryCode=US&classificationName=Music&apikey=${apiKey}`)
        .then((response) => {
          console.log("random events index", response);
          this.randomTickets = response.data._embedded.events;
          console.log(this.randomTickets);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["No Search Results Available.."];
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
    showRandomTicket: function (randomTicket) {
      this.currentTicket = randomTicket;
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
