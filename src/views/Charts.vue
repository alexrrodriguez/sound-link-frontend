<template>
  <div class="charts">
    <h1>{{ message }}</h1>
    <hr />
    <h1>Top 50 Artist Chart</h1>
    <hr />
    <div v-for="artist in artists" :key="artist.id">
      <h2>{{ artist.name }}</h2>
      <p>
        <a :href="artist.url">{{ artist.url }}</a>
      </p>
      <h5>Playcount:</h5>
      <p>{{ artist.playcount }}</p>
      <h5>Listeners:</h5>
      <p>{{ artist.listeners }}</p>
      <hr />
    </div>
    <h1>Top 50 Tracks Chart</h1>
    <hr />
    <div v-for="track in tracks" :key="track.id">
      <h2>Title: {{ track.name }}</h2>
      <h3>Artist: {{ track.artist.name }}</h3>
      <p>
        <a :href="track.url">{{ track.url }}</a>
      </p>
      <h5>Playcount:</h5>
      <p>{{ track.playcount }}</p>
      <h5>Listeners:</h5>
      <p>{{ track.listeners }}</p>
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
      message: "Todays Top Charts",
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
