<template>
  <div>
    <h1>{{ message }}</h1>
    <hr />
    <h3>Search Artist:</h3>
    <input type="text" v-model="artistSearch" placeholder="enter artist name.." />
    <br />
    <br />
    <button @click="artistInfo">Search!</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Search for Artist!",
      artists: [],
      artistSearch: "",
    };
  },
  methods: {
    artistInfo: function () {
      let apiKey = process.env.VUE_APP_LASTFM_TOKEN;
      axios
        .get(
          `https://ws.audioscrobbler.com/2.0/?method=artist.getinfo&artist=${this.artistSearch}&api_key=${apiKey}&format=json`
        )
        .then((response) => {
          console.log("artist search", response);
          this.artists = response.data.artist;
          console.log(this.artists);
        });
    },
  },
};
</script>
