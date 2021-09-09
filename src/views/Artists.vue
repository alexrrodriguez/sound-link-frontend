<template>
  <div>
    <h1>{{ message }}</h1>
    <hr />
    <h3>Search Artist:</h3>
    <input type="text" v-model="artistSearch" placeholder="enter artist name.." />
    <br />
    <br />
    <button @click="artistInfo">Search!</button>
    <br />
    <hr />
    <div v-if="artists.length !== 0">
      <h2>{{ artists.name }}</h2>
      <p>
        <a :href="artists.url">{{ artists.url }}</a>
      </p>
      <h5>Tags:</h5>
      <div v-for="tag in tags" :key="tag.id">
        <p>{{ tag.name }}</p>
      </div>
      <button v-on:click="showBio">Show Biography</button>
      <dialog id="bio-details">
        <form method="dialog">
          <h3>Biography:</h3>
          <p>{{ bio }}</p>
          <a :href="wiki">{{ artists.name }} Wikepedia Page</a>
          <br />
          <button>Close</button>
        </form>
      </dialog>
    </div>
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
      tags: [],
      summary: "",
      bio: "",
      wiki: "",
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
          this.tags = response.data.artist.tags.tag;
          console.log(this.tags);
          this.summary = response.data.artist.bio.summary;
          console.log(this.summary);
          this.bio = response.data.artist.bio.content;
          console.log(this.bio);
          this.wiki = response.data.artist.bio.links.link.href;
        });
    },
    showBio: function () {
      document.querySelector("#bio-details").showModal();
    },
  },
};
</script>
