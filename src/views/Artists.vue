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
      <p>{{ summary }}</p>
      <button v-on:click="showBio">Show Biography</button>
      <h5>Tags:</h5>
      <div v-for="tag in tags" :key="tag.id">
        <p>{{ tag.name }}</p>
      </div>
      <dialog id="bio-details">
        <form method="dialog">
          <h3>Biography:</h3>
          <p>{{ bio }}</p>
          <a :href="wiki">{{ artists.name }} Wikepedia Page</a>
          <br />
          <br />
          <button>Close</button>
        </form>
      </dialog>
      <hr />
      <h4>Albums:</h4>
      <div v-for="album in albums" :key="album.id">
        <h5>
          <a :href="album.url">{{ album.name }}</a>
        </h5>
      </div>
      <hr />
      <h4>Similar Artists:</h4>

      <div v-for="similar in similars" :key="similar.id">
        <h5>{{ similar.name }}</h5>
        <button @click="artistInfo" v-on:click="artistSearch = similar.name">Click to add Artist to Search Bar</button>
      </div>
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
      message: "Search for Artist!",
      artists: [],
      artistSearch: "",
      tags: [],
      summary: "",
      bio: "",
      wiki: "",
      similars: [],
      albums: [],
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
          console.log(this.wiki);
          this.similars = response.data.artist.similar.artist;
          console.log(this.similars);
          window.scrollTo(0, 0);
        });
      axios
        .get(
          `https://ws.audioscrobbler.com/2.0/?method=artist.gettopalbums&artist=${this.artistSearch}&api_key=${apiKey}&format=json`
        )
        .then((response) => {
          console.log("artist albums", response);
          this.albums = response.data.topalbums.album;
          console.log(this.albums);
        });
    },
    showBio: function () {
      document.querySelector("#bio-details").showModal();
    },
  },
};
</script>
