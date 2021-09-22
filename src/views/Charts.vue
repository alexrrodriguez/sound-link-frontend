<template>
  <div class="charts">
    <section class="section-bg filter-section events">
      <div class="container">
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <h2 class="chart-title-header">{{ message }}</h2>
          </div>
        </div>
      </div>
    </section>
    <div class="chart-wrapper">
      <div class="artist-chart">
        <section class="section-bg filter-section events">
          <div class="container">
            <div class="row">
              <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
                <h2 class="chart-title">Top 50 Artist Chart</h2>
                <hr />
                <div class="row justify-content-center no-gutters match-height chart-list-container">
                  <div class="col-12 col-sm-12 col-md-12 col-lg-8 event-rap event-anniversary event-romance">
                    <div class="chart-list-container">
                      <ol class="chart-list">
                        <li v-for="artist in artists" :key="artist.id">
                          <div>
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
      <div class="track-chart">
        <section class="section-bg filter-section events">
          <div class="container">
            <div class="row">
              <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
                <h2 class="chart-title">Top 50 Tracks Chart</h2>
                <hr />
                <div class="row justify-content-center match-height">
                  <div class="col-12 col-sm-12 col-md-12 col-lg-8 event-card event-rap event-anniversary event-romance">
                    <div class="chart-list-container">
                      <ol class="chart-list">
                        <li v-for="track in tracks" :key="track.id">
                          <h2 class="chart-name">{{ track.name }}</h2>
                          <h1>By</h1>
                          <h4 class="artist-name-track">{{ track.artist.name }}</h4>
                          <br />
                          <h5>Playcount:</h5>
                          <p>{{ track.playcount }}</p>
                          <h5>Listeners:</h5>
                          <p>{{ track.listeners }}</p>
                          <button class="btn btn-primary ripple"><a :href="track.url">More Info</a></button>

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
</template>

<style>
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
  margin: 1%;
  text-align: center;
  display: inline-block;
  width: 48%;
}
.track-chart {
  margin: 1%;
  text-align: center;
  display: inline-block;
  width: 48%;
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
    this.tracksChart();
  },
  methods: {
    artistChart: function () {
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
