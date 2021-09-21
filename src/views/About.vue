<template>
  <div class="about">
    <!-- Page title section -->
    <section class="section-bg after-header">
      <div class="container-fluid">
        <div class="row">
          <div class="col-lg-12 text-center">
            <h2>Contact US</h2>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact section -->
    <section class="section-bg single-content-right">
      <div class="row no-gutters">
        <!-- Left side -->
        <div class="col-lg-6">
          <div class="left-side">
            <!-- Contact map -->
            <div class="contact-image" id="map"></div>
          </div>
        </div>
        <!-- End of left side -->

        <!-- Right side -->
        <div class="col-lg-6">
          <div class="right-side purple">
            <div class="row no-gutters">
              <!-- Contact details -->
              <div class="col-lg-12">
                <h1>Contact details</h1>
                <p>
                  You can contact us anytime between 9:00 to 18:00 (GMT+2). We will be happy to help you out with any
                  questions that you have related to this theme!
                </p>
                <ul class="address-details">
                  <li>
                    <img src="img/phoneIcon.svg" alt="Contact Phone" width="30" height="30" />
                    +371 222-333-22
                  </li>
                  <li>
                    <img src="img/mailIcon.svg" alt="Contact Email" width="30" height="30" />
                    info@soundlink.com
                  </li>
                  <li>
                    <img src="img/addressIcon.svg" alt="Contact Address" width="30" height="30" />
                    Chicago, Illinois
                  </li>
                </ul>
                <h1>Leave your message</h1>
                <p>We are happy to hear from you, please fill out the below form to contact us!</p>
              </div>
            </div>

            <!-- Contact form -->
            <div class="row">
              <div class="col-md-4">
                <div class="solid-form">
                  <input type="text" id="name" class="form-control" />
                  <label for="name">Name</label>
                </div>
              </div>
              <div class="col-md-6">
                <div class="solid-form">
                  <input type="text" id="email-contact" class="form-control" />
                  <label for="email-contact">Email</label>
                </div>
              </div>
              <div class="col-md-4">
                <div class="solid-form">
                  <input type="text" id="subject" class="form-control" />
                  <label for="subject">Subject</label>
                </div>
              </div>
              <div class="col-md-10">
                <div class="solid-form">
                  <input type="text" id="messege" class="form-control" />
                  <label for="messege">Message</label>
                </div>
              </div>
              <div class="col-md-10 text-right">
                <button type="button" class="btn btn-rounded2 ripple">Send</button>
              </div>
            </div>
            <!-- End of contact form -->
          </div>
        </div>
        <!-- End of Right side -->
      </div>
    </section>
  </div>
</template>

<style>
.contact-image {
  content: url(https://iitcoauim.files.wordpress.com/2012/05/021.jpg);
}
</style>
