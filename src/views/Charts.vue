<template>
  <div class="charts">
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
              <h2 class="chart-title-header">{{ message }}</h2>
              <br />
              <div>
                <h1 class="chart-title-header" for="artist">Top 50 Artists Chart</h1>
                <br />
                <input @click="artistChart()" type="radio" id="artist" name="drone" checked />
              </div>
              <br />
              <div>
                <h1 class="chart-title-header" for="tracks">Top 50 Tracks Chart</h1>
                <br />
                <input @click="tracksChart()" type="radio" id="tracks" name="drone" />
              </div>
            </div>
          </div>
        </div>
      </section>
      <div class="chart-wrapper">
        <div v-if="artists.length != 0" class="artist-chart">
          <section class="section-bg filter-section events">
            <div class="container">
              <div class="row">
                <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
                  <h2 class="chart-title">Top 50 Artists Chart</h2>
                  <hr />
                  <div class="row justify-content-center no-gutters match-height chart-list-container">
                    <div class="col-12 col-sm-12 col-md-12 col-lg-8 event-rap event-anniversary event-romance">
                      <div class="chart-list-container">
                        <ol class="chart-list">
                          <li v-for="artist in artists" :key="artist.id">
                            <div class="chart-div">
                              <h2 class="chart-name">{{ artist.name }}</h2>

                              <h5>Playcount:</h5>
                              <p>{{ artist.playcount }}</p>
                              <h5>Listeners:</h5>
                              <p>{{ artist.listeners }}</p>
                              <button class="btn btn-primary ripple"><a :href="artist.url">More Info</a></button>

                              <hr />
                            </div>
                          </li>
                        </ol>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
        <div v-if="tracks.length != 0" class="track-chart">
          <section class="section-bg filter-section events">
            <div class="container">
              <div class="row">
                <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
                  <h2 class="chart-title">Top 50 Tracks Chart</h2>
                  <hr />
                  <div class="row justify-content-center match-height">
                    <div
                      class="col-12 col-sm-12 col-md-12 col-lg-8 event-card event-rap event-anniversary event-romance"
                    >
                      <div class="chart-list-container">
                        <ol class="chart-list">
                          <li v-for="track in tracks" :key="track.id">
                            <div class="chart-div">
                              <h2 class="chart-name">{{ track.name }}</h2>
                              <h1>By</h1>
                              <h4 class="artist-name-track">{{ track.artist.name }}</h4>
                              <br />
                              <h5>Playcount:</h5>
                              <p>{{ track.playcount }}</p>
                              <h5>Listeners:</h5>
                              <p>{{ track.listeners }}</p>
                              <button class="btn btn-primary ripple"><a :href="track.url">More Info</a></button>
                            </div>
                            <hr />
                          </li>
                        </ol>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
input[type="radio"] {
  transform: scale(2);
}
.chart-div:hover {
  background: lightblue;
}
.chart-name {
  font-size: 3rem !important;
}
.chart-list-container {
  overflow: auto;
}
.artist-name-track {
  color: white;
}
.chart-title-header {
  text-align: center;
  font-size: 3rem;
}
.chart-title {
  text-align: center;
  font-size: 3.5rem;
}
.chart-wrapper {
  position: relative;
}
.artist-chart {
  margin: 4%;
  text-align: center;
  /* display: inline-block;
  width: 48%; */
}
.track-chart {
  margin: 4%;
  text-align: center;
  /* display: inline-block;
  width: 48%; */
}
.more-info-artist {
  color: blue;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Weekly Top Charts",
      artists: [],
      tracks: [],
    };
  },
  created: function () {
    this.artistChart();
    // this.tracksChart();
  },
  methods: {
    artistChart: function () {
      this.tracks = [];
      let apiKey = process.env.VUE_APP_LASTFM_TOKEN;
      axios
        .get(`https://ws.audioscrobbler.com/2.0/?method=chart.gettopartists&api_key=${apiKey}&format=json`)
        .then((response) => {
          console.log("artist chart", response);
          this.artists = response.data.artists.artist;
          // console.log(this.artists);
        });
    },
    tracksChart: function () {
      this.artists = [];
      let apiKey = process.env.VUE_APP_LASTFM_TOKEN;
      axios
        .get(`https://ws.audioscrobbler.com/2.0/?method=chart.gettoptracks&api_key=${apiKey}&format=json`)
        .then((response) => {
          console.log("tracks chart", response);
          this.tracks = response.data.tracks.track;
        });
    },
  },
};
</script>
