<template>
  <div class="profile-page">
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
              <h2>Profile</h2>
              <div class="col-lg-6 col-md-6 col-sm-6 col-12 gallery-card profile-card">
                <div class="card hoverable">
                  <div class="view overlay img-frame">
                    <img :src="user.image" :alt="user.name" />
                  </div>
                  <div class="card-block">
                    <h4 class="card-title">{{ user.name }}</h4>
                    <p class="card-text">{{ user.email }}</p>
                    <br />
                  </div>
                </div>
              </div>
              <br />
              <hr />
              <h2>{{ user.name }} Concert Schedule:</h2>
              <hr />
              <div class="row justify-content-center no-gutters match-height">
                <div
                  v-for="concert in concerts"
                  :key="concert.id"
                  class="col-12 col-sm-12 col-md-6 col-lg-4 event-card event-rap event-anniversary event-romance"
                >
                  <!--Card-->
                  <div class="card hoverable">
                    <div class="view overlay img-frame">
                      <img v-bind:src="concert.image" v-bind:alt="concert.name" />
                      <a v-on:click="showTicket(concert)">
                        <div class="img-mask"></div>
                      </a>
                    </div>
                    <!--Card content-->
                    <div class="card-block">
                      <!--Title-->
                      <h4 class="card-title">{{ concert.name }}</h4>
                      <!--Text-->
                      <h5 class="card-text">{{ concert.city }}</h5>
                      <h5 class="card-text">
                        {{ concert.venue }}
                      </h5>
                      <div class="filter-card-details">
                        <ul>
                          <li>
                            <i class="fa fa-map-marker" aria-hidden="true"></i>
                            {{ concert.address }},
                          </li>
                          <li>
                            <i class="fa fa-calendar" aria-hidden="true"></i>
                            {{ concert.date }}
                          </li>
                          <li>
                            <i class="fa fa-clock-o" aria-hidden="true"></i>
                            {{ concert.time }}
                          </li>
                        </ul>
                      </div>
                      <hr />
                      <h1 class="event-info-text">Click for Event / Ticket Info</h1>
                      <br />
                    </div>
                    <!--/.Card content-->
                  </div>
                  <!--/.Card-->
                </div>
              </div>
              <dialog type="button" id="ticket-details">
                <form method="dialog">
                  <!-- Large card -->
                  <div class="col-xl-12 col-md-12 col-large-card">
                    <div class="card hoverable large-card">
                      <div class="view overlay img-frame">
                        <img v-bind:src="currentTicket.image" />
                      </div>
                      <!--Card content-->
                      <div class="card-block">
                        <!--Title-->
                        <h4 class="card-title">{{ currentTicket.name }}</h4>
                        <!--Text-->
                        <h1>{{ currentTicket.city }}</h1>
                        <h1>
                          {{ currentTicket.venue }}
                        </h1>
                        <div class="filter-card-details">
                          <ul>
                            <li>
                              <i class="fa fa-map-marker" aria-hidden="true"></i>
                              {{ currentTicket.address }}
                            </li>
                            <li>
                              <i class="fa fa-calendar" aria-hidden="true"></i>
                              {{ currentTicket.date }}
                            </li>
                            <li>
                              <i class="fa fa-clock-o" aria-hidden="true"></i>
                              {{ currentTicket.time }}
                            </li>
                          </ul>
                        </div>
                        <p>
                          <button class="btn btn-primary ripple">
                            <a :href="currentTicket.tickets">Buy Tickets!</a>
                          </button>
                        </p>
                        <hr />
                        <button class="btn btn-danger ripple" v-on:click="destroyUserConcert(currentTicket)">
                          Remove Concert From Schedule
                        </button>
                        <br />
                        <br />
                        <button class="btn btn-warning ripple">Back</button>
                        <br />
                        <br />
                      </div>
                    </div>
                    <!--/.Card large-->
                  </div>
                  <!--/.col large-->
                </form>
              </dialog>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style>
.profile-card {
  margin: 0 auto;
  float: none;
}
</style>

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
      axios.get("/users").then((response) => {
        console.log("user index", response);
        this.user = response.data;
      });
    },
    indexUserConcerts: function () {
      axios.get("/user_concerts").then((response) => {
        console.log("user concerts index", response);
        this.concerts = response.data;
      });
    },
    showTicket: function (concert) {
      this.currentTicket = concert;
      console.log("current ticket", this.currentTicket);
      document.querySelector("#ticket-details").showModal();
    },
    destroyUserConcert: function (currentTicket) {
      axios.delete("/user_concerts/" + currentTicket.id).then((response) => {
        console.log("user concert destroy", response);
        window.location.reload();
      });
    },
  },
};
</script>
