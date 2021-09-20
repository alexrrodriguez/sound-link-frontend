<template>
  <div id="app">
    <!-- Navigation -->
    <nav class="navbar navbar-toggleable-md navbar-light navbar-bg">
      <!-- Mobile nav toggler -->
      <button
        class="navbar-toggler navbar-toggler-right"
        type="button"
        data-toggle="collapse"
        data-target="#mob-nav"
        aria-controls="mob-nav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <!-- Musicians LOGO -->
      <a class="navbar-brand logo ripple" href="/">
        <h1 class="sound-link-header">Sound Link</h1>
        <!-- <img src="img/logo.svg" alt="Musicians -- Premium HTML template" /> -->
      </a>

      <!-- Menu -->
      <div class="collapse navbar-collapse" id="mob-nav">
        <!-- Main Menu -->
        <ul class="navbar-nav mx-auto">
          <li class="nav-item text-center">
            <a class="nav-link active" aria-haspopup="true" aria-expanded="false" href="/">Home</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link" aria-haspopup="true" aria-expanded="false" href="/charts">Charts</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link" aria-haspopup="true" aria-expanded="false" href="/artists">Artists</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link" aria-haspopup="true" aria-expanded="false" href="/profile" v-if="isLoggedIn()">
              Profile
            </a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link" aria-haspopup="true" aria-expanded="false" href="/about">Contact</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link ripple" href="/login" v-if="!isLoggedIn()">Login</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link ripple" href="/logout" v-if="isLoggedIn()">Logout</a>
          </li>
          <li class="nav-item text-center">
            <a class="nav-link ripple" href="/signup" v-if="!isLoggedIn()">Signup</a>
          </li>
        </ul>

        <!-- Social menu -->
        <ul class="navbar-nav social-nav">
          <li class="nav-item">
            <a class="nav-link ripple" href="#">
              <i class="fa fa-facebook"></i>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link ripple" href="#">
              <i class="fa fa-twitter"></i>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link ripple" href="#">
              <i class="fa fa-instagram"></i>
            </a>
          </li>
        </ul>
      </div>
    </nav>

    <!-- <div id="nav">
      <router-link to="/">Home</router-link>
      |
      <router-link to="/charts">Top 50 Charts</router-link>
      |
      <router-link to="/artists">Search Artists</router-link>
      |
      <router-link to="/profile" v-if="isLoggedIn()">Profile Page</router-link>
      |
      <router-link to="/about">About</router-link>
      |
      <router-link to="/login" v-if="!isLoggedIn()">Login</router-link>
      |
      <router-link to="/logout" v-if="isLoggedIn()">Logout</router-link>
      |
      <router-link to="/signup" v-if="!isLoggedIn()">Signup</router-link>
    </div> -->
    <router-view />
    <!-- General info section -->
    <section class="general-info">
      <div class="container">
        <div class="row">
          <!-- General info widget 1 -->
          <div class="col-lg-6 general-info-block hidden-sm-down">
            <h2>
              Musicians
              <br />
              Worspress theme
            </h2>
            <p>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent sit amet sem id erat commodo vulputate.
              Sed a commodo quam. Curabitur id ante vel elit pellentesque eleifend vitae vel risus. Pellentesque tempus
              ex sit amet nisl rutrum placerat. Sed vel mattis sem. Sed vel accumsan magna. Duis enim sapien, cursus ac
              enim sed, cursus consectetur lorem. Integer egestas vulputate turpis, rhoncus ornare orci rhoncus eget.
              Sed lobortis sem eget accumsan imperdiet. Vivamus aliquam quis urna in laoreet. Nunc rhoncus eleifend
              diam, porttitor interdum elit commodo dictum
            </p>
          </div>

          <!-- General info widget 2 -->
          <div class="col-lg-6 general-info-block">
            <!-- Subscribe to newsletter form -->
            <form>
              <div class="form-group">
                <label for="email">Subscribe to newsletter</label>
                <div class="input-group">
                  <input class="solid-input form" type="email" id="email" placeholder="E-mail adress" />
                  <span class="input-group-btn">
                    <button class="btn btn-ok btn-okHome" type="button">OK</button>
                  </span>
                </div>
              </div>
            </form>

            <!-- Social media footer -->
            <h4>Follow us on social media</h4>
            <ul class="social-media">
              <li>
                <a class="ripple" href="#"><i class="fa fa-facebook"></i></a>
              </li>
              <li>
                <a class="ripple" href="#"><i class="fa fa-twitter"></i></a>
              </li>
              <li>
                <a class="ripple" href="#"><i class="fa fa-instagram"></i></a>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- Scroll to top button -->
      <button type="button" class="btn btn-fab btn-back" id="toTop">
        <img src="img/arrow-up.svg" alt="Scroll to top" />
      </button>
    </section>
    <!-- Footer section -->
    <footer class="section-bg">
      <div class="footer-copyright">
        <h6>
          All rights reserved
          <a href="http://solid-themes.com" target="_blank">solid-themes</a>
          © 2017
        </h6>
      </div>
    </footer>
  </div>
</template>

<style>
.header-welcome {
  font-weight: lighter;
}
.header-title {
  font-style: italic;
  font-weight: bold;
}
.sound-link-header {
  margin: 6px 30px 0 20px;
  color: #28dde0;
  font-weight: bold;
  font-size: 2em;
}
</style>

<script>
export default {
  methods: {
    isLoggedIn: function () {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>
