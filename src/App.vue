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
            <a class="nav-link ripple" href="https://www.facebook.com">
              <i class="fa fa-facebook"></i>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link ripple" href="https://www.twitter.com">
              <i class="fa fa-twitter"></i>
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link ripple" href="https://www.instagram.com">
              <i class="fa fa-instagram"></i>
            </a>
          </li>
        </ul>
      </div>
    </nav>
    <router-view />
    <!-- General info section -->
    <section class="general-info">
      <div class="container">
        <div class="row">
          <!-- General info widget 1 -->
          <div class="col-lg-6 general-info-block hidden-sm-down">
            <h2>Sound Link</h2>
            <p>
              Sound Link is Website that allows users to search for music events by location and genre. Users are able
              to sign in and add music events to their schedule on their profile page. Sound Link has Weekly Top Charts
              for artists and tracks. Users can also find more information about their favorite artists by searching on
              the artists page. Sign up now and start searching for music events to add to your schedule!
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
                <a class="ripple" href="https://www.facebook.com"><i class="fa fa-facebook"></i></a>
              </li>
              <li>
                <a class="ripple" href="https://www.twitter.com"><i class="fa fa-twitter"></i></a>
              </li>
              <li>
                <a class="ripple" href="https://www.instagram.com"><i class="fa fa-instagram"></i></a>
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
          Created By
          <a href="https://alexanderrodriguez.dev/" target="_blank">Alexander Rodriguez</a>
          © 2021
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
