<template>
  <div>
    <section class="section-bg filter-section events">
      <div class="container">
        <div class="row home-search-header">
          <div class="col-md-12 col-sm-12 col-xs-12 section-main-title">
            <h1 class="chart-title-header">Search Artist:</h1>
            <input
              class="solid-input form artist-input"
              type="text"
              id="blog-search"
              placeholder="enter artist name.."
              v-model="artistSearch"
            />
            <br />
            <br />
            <button @click="artistInfo">Search!</button>
            <br />
            <br />
            <hr />
            <ul>
              <li v-for="error in errors" :key="error">{{ error }}</li>
            </ul>
            <div v-if="artists.length !== 0">
              <br />
              <h2>{{ artists.name }}</h2>
              <p>{{ summary }}</p>
              <button v-on:click="showBio">Show Biography</button>
              <br />
              <br />
              <h5>Tags:</h5>
              <div v-for="tag in tags" :key="tag.id">
                <p>{{ tag.name }}</p>
              </div>
              <h5>
                <a :href="artists.url">Last FM Artist Page</a>
              </h5>
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
              <br />
              <h4>Albums:</h4>
              <br />
              <div v-for="album in albums" :key="album.id">
                <h5>
                  <a :href="album.url">{{ album.name }}</a>
                </h5>
              </div>
              <hr />
              <h4>Similar Artists:</h4>
              <div class="col-lg-12 col-md-12 col-sm-10">
                <div class="row no-gutters justify-content-center match-height">
                  <!-- Small Card -->
                  <div v-for="similar in similars" :key="similar.id" class="col-lg-4 col-md-4 col-sm-6 col-12">
                    <div class="card hoverable small-card">
                      <!--Card content-->
                      <div class="card-block">
                        <!--Title-->
                        <h4 class="card-title">{{ similar.name }}</h4>
                        <!--Text-->
                        <br />
                        <div class="filter-card-details">
                          <button @click="artistInfo" v-on:click="artistSearch = similar.name">
                            Add Artist to Search Bar
                          </button>
                          <br />
                          <br />
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <hr />
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
.artist-input {
  width: 35%;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Search for Artist!",
      errors: [],
      events: [],
      artists: [],
      artistSearch: "",
      tags: [],
      images: [],
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
          this.images = response.data.artist.image;
          console.log("artists images", this.images);
          this.summary = response.data.artist.bio.summary;
          console.log(this.summary);
          this.bio = response.data.artist.bio.content;
          console.log(this.bio);
          this.wiki = response.data.artist.bio.links.link.href;
          console.log(this.wiki);
          this.similars = response.data.artist.similar.artist;
          console.log(this.similars);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["No Search Results Available.."];
        });
      axios
        .get(
          `https://ws.audioscrobbler.com/2.0/?method=artist.gettopalbums&artist=${this.artistSearch}&api_key=${apiKey}&format=json`
        )
        .then((response) => {
          console.log("artist albums", response);
          this.albums = response.data.topalbums.album;
          console.log(this.albums);
          window.scrollTo(0, 0);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["No Search Results Available.."];
        });
    },
    showBio: function () {
      document.querySelector("#bio-details").showModal();
    },
  },
};
</script>
