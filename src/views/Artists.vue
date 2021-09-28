<template>
  <div>
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
              <h2 class="search-artist-header">Search Artist:</h2>
              <input
                class="solid-input form artist-input"
                type="text"
                id="blog-search"
                placeholder="enter artist name.."
                v-model="artistSearch"
              />
              <br />
              <br />
              <button class="btn btn-primary ripple" @click="artistInfo">Search!</button>
              <br />
              <br />
              <hr />
              <ul>
                <li v-for="error in errors" :key="error">{{ error }}</li>
              </ul>
              <div v-if="artists.length !== 0">
                <br />
                <h2 class="artist-name-title">{{ artists.name }}</h2>
                <p>{{ summary }}</p>
                <button class="btn btn-success ripple" v-on:click="showBio">Show Biography</button>
                <br />
                <br />
                <hr />
                <br />
                <h5 class="tag-title">Tags:</h5>
                <br />
                <div v-for="tag in tags" :key="tag.id">
                  <p>{{ tag.name }}</p>
                </div>
                <br />
                <hr />
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
                    <button class="btn btn-primary ripple">Close</button>
                  </form>
                </dialog>
                <hr />
                <br />
                <h2>Albums:</h2>
                <div v-for="album in albums" :key="album.id">
                  <h5>
                    <a :href="album.url">{{ album.name }}</a>
                  </h5>
                </div>
                <hr />
                <h2>Similar Artists:</h2>
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
                            <button
                              class="btn btn-success ripple"
                              @click="artistInfo"
                              v-on:click="artistSearch = similar.name"
                            >
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
  </div>
</template>

<style>
.tag-title {
  color: white;
  font-size: 1.5rem;
}
.artist-input {
  width: 45%;
}
.artist-name-title {
  font-weight: bold;
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
