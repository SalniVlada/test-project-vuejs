<script>
import Header from "./Header.vue";
import Footer from "./Footer.vue";

import { on, select } from "@/assets/js/utils.js";

export default {
  components: {
    Header,
    Footer,
  },

  mounted() {
    (function () {
      /**
       * Easy on scroll event listener
       */
      const onscroll = (el, listener) => {
        el.addEventListener("scroll", listener);
      };

      /**
       * Navbar links active state on scroll
       */
      let navbarlinks = select("#navbar .scrollto", true);
      const navbarlinksActive = () => {
        let position = window.scrollY + 200;
        navbarlinks.forEach((navbarlink) => {
          if (!navbarlink.hash) return;
          let section = select(navbarlink.hash);
          if (!section) return;
          if (
            position >= section.offsetTop &&
            position <= section.offsetTop + section.offsetHeight
          ) {
            navbarlink.classList.add("active");
          } else {
            navbarlink.classList.remove("active");
          }
        });
      };
      window.addEventListener("load", navbarlinksActive);
      onscroll(document, navbarlinksActive);

      /**
       * Scrolls to an element with header offset
       */
      const scrollto = (el) => {
        let header = select("#header");
        let offset = header.offsetHeight;

        if (!header.classList.contains("header-scrolled")) {
          offset -= 16;
        }

        let elementPos = select(el).offsetTop;
        window.scrollTo({
          top: elementPos - offset,
          behavior: "smooth",
        });
      };

      /**
       * Toggle .header-scrolled class to #header when page is scrolled
       */
      let selectHeader = select("#header");
      if (selectHeader) {
        const headerScrolled = () => {
          if (window.scrollY > 100) {
            selectHeader.classList.add("header-scrolled");
          } else {
            selectHeader.classList.remove("header-scrolled");
          }
        };
        window.addEventListener("load", headerScrolled);
        onscroll(document, headerScrolled);
      }

      /**
       * Back to top button
       */
      let backtotop = select(".back-to-top");
      if (backtotop) {
        const toggleBacktotop = () => {
          if (window.scrollY > 100) {
            backtotop.classList.add("active");
          } else {
            backtotop.classList.remove("active");
          }
        };
        window.addEventListener("load", toggleBacktotop);
        onscroll(document, toggleBacktotop);
      }

      /**
       * Scrool with ofset on links with a class name .scrollto
       */
      on(
        "click",
        ".scrollto",
        function (e) {
          if (select(this.hash)) {
            e.preventDefault();

            let navbar = select("#navbar");
            if (navbar.classList.contains("navbar-mobile")) {
              navbar.classList.remove("navbar-mobile");
              let navbarToggle = select(".mobile-nav-toggle");
              navbarToggle.classList.toggle("bi-list");
              navbarToggle.classList.toggle("bi-x");
            }
            scrollto(this.hash);
          }
        },
        true
      );

      /**
       * Scroll with ofset on page load with hash links in the url
       */
      window.addEventListener("load", () => {
        if (window.location.hash) {
          if (select(window.location.hash)) {
            scrollto(window.location.hash);
          }
        }
      });

      /**
       * Preloader
       */
      let preloader = select("#preloader");
      preloader.classList.add("hidden");

      /**
       * Hero carousel indicators
       */
      let heroCarouselIndicators = select("#hero-carousel-indicators");
      let heroCarouselItems = select("#heroCarousel .carousel-item", true);

      heroCarouselItems.forEach((item, index) => {
        index === 0
          ? (heroCarouselIndicators.innerHTML +=
              "<li data-bs-target='#heroCarousel' data-bs-slide-to='" +
              index +
              "' class='active'></li>")
          : (heroCarouselIndicators.innerHTML +=
              "<li data-bs-target='#heroCarousel' data-bs-slide-to='" +
              index +
              "'></li>");
      });

      /**
       * Porfolio isotope and filter
       */
      window.addEventListener("load", () => {
        let portfolioContainer = select(".portfolio-container");
        if (portfolioContainer) {
          let portfolioIsotope = new Isotope(portfolioContainer, {
            itemSelector: ".portfolio-item",
            layoutMode: "fitRows",
          });

          let portfolioFilters = select("#portfolio-flters li", true);

          on(
            "click",
            "#portfolio-flters li",
            function (e) {
              e.preventDefault();
              portfolioFilters.forEach(function (el) {
                el.classList.remove("filter-active");
              });
              this.classList.add("filter-active");

              portfolioIsotope.arrange({
                filter: this.getAttribute("data-filter"),
              });
            },
            true
          );
        }
      });

      /**
       * Initiate portfolio lightbox
       */
      const portfolioLightbox = GLightbox({
        selector: ".portfolio-lightbox",
      });

      /**
       * Testimonials slider
       */
      new Swiper(".testimonials-slider", {
        speed: 600,
        loop: true,
        autoplay: {
          delay: 5000,
          disableOnInteraction: false,
        },
        slidesPerView: "auto",
        pagination: {
          el: ".swiper-pagination",
          type: "bullets",
          clickable: true,
        },
      });
    })();
  },
};
</script>

<template>
  <Header />

  <!-- ======= hero Section ======= -->
  <section id="hero">
    <div class="hero-container">
      <div
        id="heroCarousel"
        class="carousel slide carousel-fade"
        data-bs-ride="carousel"
        data-bs-interval="5000"
      >
        <ol id="hero-carousel-indicators" class="carousel-indicators"></ol>

        <div class="carousel-inner" role="listbox">
          <div
            class="carousel-item active"
            style="background-image: url(src/assets/img/hero-carousel/1.jpg)"
          >
            <div class="carousel-container">
              <div class="container">
                <h2 class="animate__animated animate__fadeInDown">
                  The Best Business Information
                </h2>
                <p class="animate__animated animate__fadeInUp">
                  We're In The Business Of Helping You Start Your Business
                </p>
                <a
                  href="#about"
                  class="btn-get-started scrollto animate__animated animate__fadeInUp"
                  >Get Started</a
                >
              </div>
            </div>
          </div>

          <div
            class="carousel-item"
            style="background-image: url(src/assets/img/hero-carousel/2.jpg)"
          >
            <div class="carousel-container">
              <div class="container">
                <h2 class="animate__animated animate__fadeInDown">
                  At vero eos et accusamus
                </h2>
                <p class="animate__animated animate__fadeInUp">
                  Helping Business Security & Peace of Mind for Your Family
                </p>
                <a
                  href="#about"
                  class="btn-get-started scrollto animate__animated animate__fadeInUp"
                  >Get Started</a
                >
              </div>
            </div>
          </div>

          <div
            class="carousel-item"
            style="background-image: url(./src/assets/img/hero-carousel/3.jpg)"
          >
            <div class="carousel-container">
              <div class="container">
                <h2 class="animate__animated animate__fadeInDown">
                  Temporibus autem quibusdam
                </h2>
                <p class="animate__animated animate__fadeInUp">
                  Beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem
                </p>
                <a
                  href="#about"
                  class="btn-get-started scrollto animate__animated animate__fadeInUp"
                  >Get Started</a
                >
              </div>
            </div>
          </div>
        </div>

        <a
          class="carousel-control-prev"
          href="#heroCarousel"
          role="button"
          data-bs-slide="prev"
        >
          <span
            class="carousel-control-prev-icon bi bi-chevron-left"
            aria-hidden="true"
          ></span>
        </a>

        <a
          class="carousel-control-next"
          href="#heroCarousel"
          role="button"
          data-bs-slide="next"
        >
          <span
            class="carousel-control-next-icon bi bi-chevron-right"
            aria-hidden="true"
          ></span>
        </a>
      </div>
    </div>
  </section>
  <!-- End Hero Section -->

  <main id="main">
    <!-- ======= About Section ======= -->
    <div id="about" class="about-area area-padding">
      <div class="container">
        <div class="row">
          <div class="col-md-12 col-sm-12 col-xs-12">
            <div class="section-headline text-center">
              <h2>About eBusiness</h2>
            </div>
          </div>
        </div>
        <div class="row">
          <!-- single-well start-->
          <div class="col-md-6 col-sm-6 col-xs-12">
            <div class="well-left">
              <div class="single-well">
                <a href="#">
                  <img src="src/assets/img/about/1.jpg" alt="" />
                </a>
              </div>
            </div>
          </div>
          <!-- single-well end-->
          <div class="col-md-6 col-sm-6 col-xs-12">
            <div class="well-middle">
              <div class="single-well">
                <a href="#">
                  <h4 class="sec-head">project Maintenance</h4>
                </a>
                <p>
                  Redug Lagre dolor sit amet, consectetur adipisicing elit.
                  Itaque quas officiis iure aspernatur sit adipisci quaerat unde
                  at nequeRedug Lagre dolor sit amet, consectetur adipisicing
                  elit. Itaque quas officiis iure
                </p>
                <ul>
                  <li><i class="bi bi-check"></i> Interior design Package</li>
                  <li><i class="bi bi-check"></i> Building House</li>
                  <li>
                    <i class="bi bi-check"></i> Reparing of Residentail Roof
                  </li>
                  <li>
                    <i class="bi bi-check"></i> Renovaion of Commercial Office
                  </li>
                  <li><i class="bi bi-check"></i> Make Quality Products</li>
                </ul>
              </div>
            </div>
          </div>
          <!-- End col-->
        </div>
      </div>
    </div>
    <!-- End About Section -->

    <!-- ======= Services Section ======= -->
    <div id="services" class="services-area area-padding">
      <div class="container">
        <div class="row">
          <div class="col-md-12 col-sm-12 col-xs-12">
            <div class="section-headline services-head text-center">
              <h2>Our Services</h2>
            </div>
          </div>
        </div>
        <div class="row text-center">
          <!-- Start Left services -->
          <div class="col-md-4 col-sm-4 col-xs-12">
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-briefcase"></i>
                  </a>
                  <h4>Expert Coder</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
          <div class="col-md-4 col-sm-4 col-xs-12">
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-card-checklist"></i>
                  </a>
                  <h4>Creative Designer</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
          <div class="col-md-4 col-sm-4 col-xs-12">
            <!-- end col-md-4 -->
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-bar-chart"></i>
                  </a>
                  <h4>Wordpress Developer</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
          <div class="col-md-4 col-sm-4 col-xs-12">
            <!-- end col-md-4 -->
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-binoculars"></i>
                  </a>
                  <h4>Social Marketer</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
          <!-- End Left services -->
          <div class="col-md-4 col-sm-4 col-xs-12">
            <!-- end col-md-4 -->
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-brightness-high"></i>
                  </a>
                  <h4>Seo Expart</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
          <!-- End Left services -->
          <div class="col-md-4 col-sm-4 col-xs-12">
            <!-- end col-md-4 -->
            <div class="about-move">
              <div class="services-details">
                <div class="single-services">
                  <a class="services-icon" href="#">
                    <i class="bi bi-calendar4-week"></i>
                  </a>
                  <h4>24/7 Support</h4>
                  <p>
                    will have to make sure the prototype looks finished by
                    inserting text or photo.make sure the prototype looks
                    finished by.
                  </p>
                </div>
              </div>
              <!-- end about-details -->
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Services Section -->

    <!-- ======= Team Section ======= -->
    <div id="team" class="our-team-area area-padding">
      <div class="container">
        <div class="row">
          <div class="col-md-12 col-sm-12 col-xs-12">
            <div class="section-headline text-center">
              <h2>Our special Team</h2>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-3 col-sm-3 col-xs-12">
            <div class="single-team-member">
              <div class="team-img">
                <a href="#">
                  <img src="src/assets/img/team/1.jpg" alt="" />
                </a>
                <div class="team-social-icon text-center">
                  <ul>
                    <li>
                      <a href="#">
                        <i class="bi bi-facebook"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-twitter"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-instagram"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class="team-content text-center">
                <h4>Jhon Mickel</h4>
                <p>Seo</p>
              </div>
            </div>
          </div>
          <!-- End column -->
          <div class="col-md-3 col-sm-3 col-xs-12">
            <div class="single-team-member">
              <div class="team-img">
                <a href="#">
                  <img src="src/assets/img/team/2.jpg" alt="" />
                </a>
                <div class="team-social-icon text-center">
                  <ul>
                    <li>
                      <a href="#">
                        <i class="bi bi-facebook"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-twitter"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-instagram"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class="team-content text-center">
                <h4>Andrew Arnold</h4>
                <p>Web Developer</p>
              </div>
            </div>
          </div>
          <!-- End column -->
          <div class="col-md-3 col-sm-3 col-xs-12">
            <div class="single-team-member">
              <div class="team-img">
                <a href="#">
                  <img src="src/assets/img/team/3.jpg" alt="" />
                </a>
                <div class="team-social-icon text-center">
                  <ul>
                    <li>
                      <a href="#">
                        <i class="bi bi-facebook"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-twitter"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-instagram"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class="team-content text-center">
                <h4>Lellien Linda</h4>
                <p>Web Design</p>
              </div>
            </div>
          </div>
          <!-- End column -->
          <div class="col-md-3 col-sm-3 col-xs-12">
            <div class="single-team-member">
              <div class="team-img">
                <a href="#">
                  <img src="src/assets/img/team/4.jpg" alt="" />
                </a>
                <div class="team-social-icon text-center">
                  <ul>
                    <li>
                      <a href="#">
                        <i class="bi bi-facebook"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-twitter"></i>
                      </a>
                    </li>
                    <li>
                      <a href="#">
                        <i class="bi bi-instagram"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
              <div class="team-content text-center">
                <h4>Jhon Powel</h4>
                <p>Seo Expert</p>
              </div>
            </div>
          </div>
          <!-- End column -->
        </div>
      </div>
    </div>
    <!-- End Team Section -->

    <!-- ======= Rviews Section ======= -->
    <div class="reviews-area">
      <div class="row g-0">
        <div class="col-lg-6">
          <img src="src/assets/img/about/2.jpg" alt="" class="img-fluid" />
        </div>
        <div class="col-lg-6 work-right-text d-flex align-items-center">
          <div class="px-5 py-5 py-lg-0">
            <h2>working with us</h2>
            <h5>
              Web Design, Ready Home, Construction and Co-operate Outstanding
              Buildings.
            </h5>
            <a href="#contact" class="ready-btn scrollto">Contact us</a>
          </div>
        </div>
      </div>
    </div>
    <!-- End Rviews Section -->

    <!-- ======= Portfolio Section ======= -->
    <div id="portfolio" class="portfolio-area area-padding fix">
      <div class="container">
        <div class="row">
          <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
            <div class="section-headline text-center">
              <h2>Our Portfolio</h2>
            </div>
          </div>
        </div>
        <div class="row wesome-project-1 fix">
          <!-- Start Portfolio -page -->
          <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
            <ul id="portfolio-flters">
              <li data-filter="*" class="filter-active">All</li>
              <li data-filter=".filter-app">App</li>
              <li data-filter=".filter-card">Card</li>
              <li data-filter=".filter-web">Web</li>
            </ul>
          </div>
        </div>

        <div class="row awesome-project-content portfolio-container">
          <!-- portfolio-item start -->
          <div
            class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-app portfolio-item"
          >
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/1.jpg" alt=""
                /></a>
                <div class="add-actions text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/1.jpg"
                    >
                      <h4>Business City</h4>
                      <span>Web Development</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->

          <!-- portfolio-item start -->
          <div class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-web">
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/2.jpg" alt=""
                /></a>
                <div class="add-actions text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/2.jpg"
                    >
                      <h4>Blue Sea</h4>
                      <span>Photosho</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->

          <!-- portfolio-item start -->
          <div class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-card">
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/3.jpg" alt=""
                /></a>
                <div class="add-actions text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/3.jpg"
                    >
                      <h4>Beautiful Nature</h4>
                      <span>Web Design</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->

          <!-- portfolio-item start -->
          <div class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-web">
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/4.jpg" alt=""
                /></a>
                <div class="add-actions text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/4.jpg"
                    >
                      <h4>Creative Team</h4>
                      <span>Web design</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->

          <!-- portfolio-item start -->
          <div class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-app">
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/5.jpg" alt=""
                /></a>
                <div class="add-actions text-center text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/5.jpg"
                    >
                      <h4>Beautiful Flower</h4>
                      <span>Web Development</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->

          <!-- portfolio-item start -->
          <div class="col-md-4 col-sm-4 col-xs-12 portfolio-item filter-web">
            <div class="single-awesome-project">
              <div class="awesome-img">
                <a href="#"
                  ><img src="src/assets//img/portfolio/6.jpg" alt=""
                /></a>
                <div class="add-actions text-center">
                  <div class="project-dec">
                    <a
                      class="portfolio-lightbox"
                      data-gallery="myGallery"
                      href="src/assets//img/portfolio/6.jpg"
                    >
                      <h4>Night Hill</h4>
                      <span>Photoshop</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- portfolio-item end -->
        </div>
      </div>
    </div>
    <!-- End Portfolio Section -->

    <!-- ======= Pricing Section ======= -->
    <div id="pricing" class="pricing-area area-padding">
      <div class="container">
        <div class="row">
          <div class="col-md-12 col-sm-12 col-xs-12">
            <div class="section-headline text-center">
              <h2>Pricing Table</h2>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-4 col-sm-4 col-xs-12">
            <div class="pri_table_list">
              <h3>
                basic <br />
                <span>$80 / month</span>
              </h3>
              <ol>
                <li class="check">
                  <i class="bi bi-check"></i><span>Online system</span>
                </li>
                <li class="check">
                  <i class="bi bi-x"></i><span>Full access</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Free apps</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Multiple slider</span>
                </li>
                <li class="cross">
                  <i class="bi bi-x"></i><span>Free domin</span>
                </li>
                <li class="cross">
                  <i class="bi bi-x"></i><span>Support unlimited</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Payment online</span>
                </li>
                <li class="check">
                  <i class="bi bi-x"></i><span>Cash back</span>
                </li>
              </ol>
              <button>sign up now</button>
            </div>
          </div>
          <div class="col-md-4 col-sm-4 col-xs-12">
            <div class="pri_table_list active">
              <span class="saleon">top sale</span>
              <h3>
                standard <br />
                <span>$110 / month</span>
              </h3>
              <ol>
                <li class="check">
                  <i class="bi bi-check"></i><span>Online system</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Full access</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Free apps</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Multiple slider</span>
                </li>
                <li class="cross">
                  <i class="bi bi-x"></i><span>Free domin</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Support unlimited</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Payment online</span>
                </li>
                <li class="cross">
                  <i class="bi bi-x"></i><span>Cash back</span>
                </li>
              </ol>
              <button>sign up now</button>
            </div>
          </div>
          <div class="col-md-4 col-sm-4 col-xs-12">
            <div class="pri_table_list">
              <h3>
                premium <br />
                <span>$150 / month</span>
              </h3>
              <ol>
                <li class="check">
                  <i class="bi bi-check"></i><span>Online system</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Full access</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Free apps</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Multiple slider</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Free domin</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Support unlimited</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Payment online</span>
                </li>
                <li class="check">
                  <i class="bi bi-check"></i><span>Cash back</span>
                </li>
              </ol>
              <button>sign up now</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Pricing Section -->

    <!-- ======= Testimonials Section ======= -->
    <div id="testimonials" class="testimonials">
      <div class="container">
        <div class="testimonials-slider swiper">
          <div class="swiper-wrapper">
            <div class="swiper-slide">
              <div class="testimonial-item">
                <img
                  src="src/assets//img/testimonials/testimonials-1.jpg"
                  class="testimonial-img"
                  alt=""
                />
                <h3>Saul Goodman</h3>
                <h4>Ceo &amp; Founder</h4>
                <p>
                  <i class="bx bxs-quote-alt-left quote-icon-left"></i>
                  Proin iaculis purus consequat sem cure digni ssim donec
                  porttitora entum suscipit rhoncus. Accusantium quam, ultricies
                  eget id, aliquam eget nibh et. Maecen aliquam, risus at
                  semper.
                  <i class="bx bxs-quote-alt-right quote-icon-right"></i>
                </p>
              </div>
            </div>
            <!-- End testimonial item -->

            <div class="swiper-slide">
              <div class="testimonial-item">
                <img
                  src="src/assets//img/testimonials/testimonials-2.jpg"
                  class="testimonial-img"
                  alt=""
                />
                <h3>Sara Wilsson</h3>
                <h4>Designer</h4>
                <p>
                  <i class="bx bxs-quote-alt-left quote-icon-left"></i>
                  Export tempor illum tamen malis malis eram quae irure esse
                  labore quem cillum quid cillum eram malis quorum velit fore
                  eram velit sunt aliqua noster fugiat irure amet legam anim
                  culpa.
                  <i class="bx bxs-quote-alt-right quote-icon-right"></i>
                </p>
              </div>
            </div>
            <!-- End testimonial item -->

            <div class="swiper-slide">
              <div class="testimonial-item">
                <img
                  src="src/assets//img/testimonials/testimonials-3.jpg"
                  class="testimonial-img"
                  alt=""
                />
                <h3>Jena Karlis</h3>
                <h4>Store Owner</h4>
                <p>
                  <i class="bx bxs-quote-alt-left quote-icon-left"></i>
                  Enim nisi quem export duis labore cillum quae magna enim sint
                  quorum nulla quem veniam duis minim tempor labore quem eram
                  duis noster aute amet eram fore quis sint minim.
                  <i class="bx bxs-quote-alt-right quote-icon-right"></i>
                </p>
              </div>
            </div>
            <!-- End testimonial item -->

            <div class="swiper-slide">
              <div class="testimonial-item">
                <img
                  src="src/assets//img/testimonials/testimonials-4.jpg"
                  class="testimonial-img"
                  alt=""
                />
                <h3>Matt Brandon</h3>
                <h4>Freelancer</h4>
                <p>
                  <i class="bx bxs-quote-alt-left quote-icon-left"></i>
                  Fugiat enim eram quae cillum dolore dolor amet nulla culpa
                  multos export minim fugiat minim velit minim dolor enim duis
                  veniam ipsum anim magna sunt elit fore quem dolore labore
                  illum veniam.
                  <i class="bx bxs-quote-alt-right quote-icon-right"></i>
                </p>
              </div>
            </div>
            <!-- End testimonial item -->

            <div class="swiper-slide">
              <div class="testimonial-item">
                <img
                  src="src/assets//img/testimonials/testimonials-5.jpg"
                  class="testimonial-img"
                  alt=""
                />
                <h3>John Larson</h3>
                <h4>Entrepreneur</h4>
                <p>
                  <i class="bx bxs-quote-alt-left quote-icon-left"></i>
                  Quis quorum aliqua sint quem legam fore sunt eram irure aliqua
                  veniam tempor noster veniam enim culpa labore duis sunt culpa
                  nulla illum cillum fugiat legam esse veniam culpa fore nisi
                  cillum quid.
                  <i class="bx bxs-quote-alt-right quote-icon-right"></i>
                </p>
              </div>
            </div>
            <!-- End testimonial item -->
          </div>
          <div class="swiper-pagination"></div>
        </div>
      </div>
    </div>
    <!-- End Testimonials Section -->

    <!-- ======= Blog Section ======= -->
    <div id="blog" class="blog-area">
      <div class="blog-inner area-padding">
        <div class="blog-overly"></div>
        <div class="container">
          <div class="row">
            <div class="col-md-12 col-sm-12 col-xs-12">
              <div class="section-headline text-center">
                <h2>Latest News</h2>
              </div>
            </div>
          </div>
          <div class="row">
            <!-- Start Left Blog -->
            <div class="col-md-4 col-sm-4 col-xs-12">
              <div class="single-blog">
                <div class="single-blog-img">
                  <a href="blog.html">
                    <img src="src/assets//img/blog/1.jpg" alt="" />
                  </a>
                </div>
                <div class="blog-meta">
                  <span class="comments-type">
                    <i class="fa fa-comment-o"></i>
                    <a href="#">13 comments</a>
                  </span>
                  <span class="date-type">
                    <i class="fa fa-calendar"></i>2016-03-05 / 09:10:16
                  </span>
                </div>
                <div class="blog-text">
                  <h4>
                    <a href="blog.html">Assumenda repud eum veniam</a>
                  </h4>
                  <p>
                    Lorem ipsum dolor sit amet conse adipis elit Assumenda repud
                    eum veniam optio modi sit explicabo nisi magnam
                    quibusdam.sit amet conse adipis elit Assumenda repud eum
                    veniam optio modi sit explicabo nisi magnam quibusdam.
                  </p>
                </div>
                <span>
                  <a href="blog.html" class="ready-btn">Read more</a>
                </span>
              </div>
              <!-- Start single blog -->
            </div>
            <!-- End Left Blog-->
            <!-- Start Left Blog -->
            <div class="col-md-4 col-sm-4 col-xs-12">
              <div class="single-blog">
                <div class="single-blog-img">
                  <a href="blog.html">
                    <img src="src/assets//img/blog/2.jpg" alt="" />
                  </a>
                </div>
                <div class="blog-meta">
                  <span class="comments-type">
                    <i class="fa fa-comment-o"></i>
                    <a href="#">130 comments</a>
                  </span>
                  <span class="date-type">
                    <i class="fa fa-calendar"></i>2016-03-05 / 09:10:16
                  </span>
                </div>
                <div class="blog-text">
                  <h4>
                    <a href="blog.html">Explicabo magnam quibusdam.</a>
                  </h4>
                  <p>
                    Lorem ipsum dolor sit amet conse adipis elit Assumenda repud
                    eum veniam optio modi sit explicabo nisi magnam
                    quibusdam.sit amet conse adipis elit Assumenda repud eum
                    veniam optio modi sit explicabo nisi magnam quibusdam.
                  </p>
                </div>
                <span>
                  <a href="blog.html" class="ready-btn">Read more</a>
                </span>
              </div>
              <!-- Start single blog -->
            </div>
            <!-- End Left Blog-->
            <!-- Start Right Blog-->
            <div class="col-md-4 col-sm-4 col-xs-12">
              <div class="single-blog">
                <div class="single-blog-img">
                  <a href="blog.html">
                    <img src="src/assets//img/blog/3.jpg" alt="" />
                  </a>
                </div>
                <div class="blog-meta">
                  <span class="comments-type">
                    <i class="fa fa-comment-o"></i>
                    <a href="#">10 comments</a>
                  </span>
                  <span class="date-type">
                    <i class="fa fa-calendar"></i>2016-03-05 / 09:10:16
                  </span>
                </div>
                <div class="blog-text">
                  <h4>
                    <a href="blog.html">Lorem ipsum dolor sit amet</a>
                  </h4>
                  <p>
                    Lorem ipsum dolor sit amet conse adipis elit Assumenda repud
                    eum veniam optio modi sit explicabo nisi magnam
                    quibusdam.sit amet conse adipis elit Assumenda repud eum
                    veniam optio modi sit explicabo nisi magnam quibusdam.
                  </p>
                </div>
                <span>
                  <a href="blog.html" class="ready-btn">Read more</a>
                </span>
              </div>
            </div>
            <!-- End Right Blog-->
          </div>
        </div>
      </div>
    </div>
    <!-- End Blog Section -->

    <!-- ======= Suscribe Section ======= -->
    <div class="suscribe-area">
      <div class="container">
        <div class="row">
          <div class="col-lg-12 col-md-12 col-sm-12 col-xs=12">
            <div class="suscribe-text text-center">
              <h3>Welcome to our eBusiness company</h3>
              <a class="sus-btn" href="#">Get A quote</a>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Suscribe Section -->

    <!-- ======= Contact Section ======= -->
    <div id="contact" class="contact-area">
      <div class="contact-inner area-padding">
        <div class="contact-overly"></div>
        <div class="container">
          <div class="row">
            <div class="col-md-12 col-sm-12 col-xs-12">
              <div class="section-headline text-center">
                <h2>Contact us</h2>
              </div>
            </div>
          </div>
          <div class="row">
            <!-- Start contact icon column -->
            <div class="col-md-4">
              <div class="contact-icon text-center">
                <div class="single-icon">
                  <i class="bi bi-phone"></i>
                  <p>
                    Call: +1 5589 55488 55<br />
                    <span>Monday-Friday (9am-5pm)</span>
                  </p>
                </div>
              </div>
            </div>
            <!-- Start contact icon column -->
            <div class="col-md-4">
              <div class="contact-icon text-center">
                <div class="single-icon">
                  <i class="bi bi-envelope"></i>
                  <p>
                    Email: info@example.com<br />
                    <span>Web: www.example.com</span>
                  </p>
                </div>
              </div>
            </div>
            <!-- Start contact icon column -->
            <div class="col-md-4">
              <div class="contact-icon text-center">
                <div class="single-icon">
                  <i class="bi bi-geo-alt"></i>
                  <p>
                    Location: A108 Adam Street<br />
                    <span>NY 535022, USA</span>
                  </p>
                </div>
              </div>
            </div>
          </div>
          <div class="row">
            <!-- Start Google Map -->
            <div class="col-md-6">
              <!-- Start Map -->
              <iframe
                src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d22864.11283411948!2d-73.96468908098944!3d40.630720240038435!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x89c24fa5d33f083b%3A0xc80b8f06e177fe62!2sNew+York%2C+NY%2C+USA!5e0!3m2!1sen!2sbg!4v1540447494452"
                width="100%"
                height="380"
                frameborder="0"
                style="border: 0"
                allowfullscreen
              ></iframe>
              <!-- End Map -->
            </div>
            <!-- End Google Map -->

            <!-- Start  contact -->
            <div class="col-md-6">
              <div class="form contact-form">
                <form
                  action="forms/contact.php"
                  method="post"
                  role="form"
                  class="php-email-form"
                >
                  <div class="form-group">
                    <input
                      type="text"
                      name="name"
                      class="form-control"
                      id="name"
                      placeholder="Your Name"
                      required
                    />
                  </div>
                  <div class="form-group mt-3">
                    <input
                      type="email"
                      class="form-control"
                      name="email"
                      id="email"
                      placeholder="Your Email"
                      required
                    />
                  </div>
                  <div class="form-group mt-3">
                    <input
                      type="text"
                      class="form-control"
                      name="subject"
                      id="subject"
                      placeholder="Subject"
                      required
                    />
                  </div>
                  <div class="form-group mt-3">
                    <textarea
                      class="form-control"
                      name="message"
                      rows="5"
                      placeholder="Message"
                      required
                    ></textarea>
                  </div>
                  <div class="my-3">
                    <div class="loading">Loading</div>
                    <div class="error-message"></div>
                    <div class="sent-message">
                      Your message has been sent. Thank you!
                    </div>
                  </div>
                  <div class="text-center">
                    <button type="submit">Send Message</button>
                  </div>
                </form>
              </div>
            </div>
            <!-- End Left contact -->
          </div>
        </div>
      </div>
    </div>
    <!-- End Contact Section -->

    <Footer />
  </main>
  <!-- End #main -->

  <div id="preloader"></div>
  <a
    href="#"
    class="back-to-top d-flex align-items-center justify-content-center"
    ><i class="bi bi-arrow-up-short"></i
  ></a>
</template>

<style scoped>
#header {
  height: 80px;
  transition: all 0.5s;
  z-index: 997;
  transition: all 0.5s;
  background: rgba(0, 0, 0, 0.5);
}

#header.header-scrolled {
  background: rgba(0, 0, 0, 0.8);
  height: 64px;
}

#header .logo h1 {
  font-size: 36px;
  margin: 0;
  padding: 0;
  line-height: 1;
  font-weight: bold;
}

#header .logo h1 span {
  color: #3ec1d5;
}

#header .logo h1 a,
#header .logo h1 a:hover {
  color: #fff;
  text-decoration: none;
}

#header .logo img {
  padding: 0;
  margin: 0;
  max-height: 40px;
}

@media (max-width: 768px) {
  #header {
    height: 64px;
    background: rgba(0, 0, 0, 0.8);
  }

  #header .logo h1 {
    font-size: 28px;
  }
}

.header-bg {
  background: url(../img/background/page-header.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top center;
  padding: 120px 0 60px;
}

.header-bg::before {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
}

@media (min-width: 1025px) {
  .header-bg {
    background-attachment: fixed;
  }
}

/*--------------------------------------------------------------
# Navigation Menu
--------------------------------------------------------------*/
/**
* Desktop Navigation 
*/
.navbar {
  padding: 0;
}

.navbar ul {
  margin: 0;
  padding: 0;
  display: flex;
  list-style: none;
  align-items: center;
}

.navbar li {
  position: relative;
}

.navbar a,
.navbar a:focus {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0 10px 30px;
  font-size: 15px;
  font-weight: 500;
  color: #fff;
  white-space: nowrap;
  transition: 0.3s;
}

.navbar a i,
.navbar a:focus i {
  font-size: 12px;
  line-height: 0;
  margin-left: 5px;
}

.navbar a:hover,
.navbar .active,
.navbar .active:focus,
.navbar li:hover > a {
  color: #3ec1d5;
}

.navbar .dropdown ul {
  display: block;
  position: absolute;
  left: 14px;
  top: calc(100% + 30px);
  margin: 0;
  padding: 10px 0;
  z-index: 99;
  opacity: 0;
  visibility: hidden;
  background: #fff;
  box-shadow: 0px 0px 30px rgba(127, 137, 161, 0.25);
  transition: 0.3s;
}

.navbar .dropdown ul li {
  min-width: 200px;
}

.navbar .dropdown ul a {
  padding: 10px 20px;
  font-size: 14px;
  text-transform: none;
  color: #0d2529;
}

.navbar .dropdown ul a i {
  font-size: 12px;
}

.navbar .dropdown ul a:hover,
.navbar .dropdown ul .active:hover,
.navbar .dropdown ul li:hover > a {
  color: #3ec1d5;
}

.navbar .dropdown:hover > ul {
  opacity: 1;
  top: 100%;
  visibility: visible;
}

.navbar .dropdown .dropdown ul {
  top: 0;
  left: calc(100% - 30px);
  visibility: hidden;
}

.navbar .dropdown .dropdown:hover > ul {
  opacity: 1;
  top: 0;
  left: 100%;
  visibility: visible;
}

@media (max-width: 1366px) {
  .navbar .dropdown .dropdown ul {
    left: -90%;
  }

  .navbar .dropdown .dropdown:hover > ul {
    left: -100%;
  }
}

/**
* Mobile Navigation 
*/
.mobile-nav-toggle {
  color: #fff;
  font-size: 28px;
  cursor: pointer;
  display: none;
  line-height: 0;
  transition: 0.5s;
}

@media (max-width: 991px) {
  .mobile-nav-toggle {
    display: block;
  }

  .navbar ul {
    display: none;
  }
}

.navbar-mobile {
  position: fixed;
  overflow: hidden;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(1, 2, 2, 0.9);
  transition: 0.3s;
  z-index: 999;
}

.navbar-mobile .mobile-nav-toggle {
  position: absolute;
  top: 15px;
  right: 15px;
}

.navbar-mobile ul {
  display: block;
  position: absolute;
  top: 55px;
  right: 15px;
  bottom: 15px;
  left: 15px;
  padding: 10px 0;
  background-color: #fff;
  overflow-y: auto;
  transition: 0.3s;
}

.navbar-mobile a,
.navbar-mobile a:focus {
  padding: 10px 20px;
  font-size: 15px;
  color: #0d2529;
}

.navbar-mobile a:hover,
.navbar-mobile .active,
.navbar-mobile li:hover > a {
  color: #3ec1d5;
}

.navbar-mobile .getstarted,
.navbar-mobile .getstarted:focus {
  margin: 15px;
}

.navbar-mobile .dropdown ul {
  position: static;
  display: none;
  margin: 10px 20px;
  padding: 10px 0;
  z-index: 99;
  opacity: 1;
  visibility: visible;
  background: #fff;
  box-shadow: 0px 0px 30px rgba(127, 137, 161, 0.25);
}

.navbar-mobile .dropdown ul li {
  min-width: 200px;
}

.navbar-mobile .dropdown ul a {
  padding: 10px 20px;
}

.navbar-mobile .dropdown ul a i {
  font-size: 12px;
}

.navbar-mobile .dropdown ul a:hover,
.navbar-mobile .dropdown ul .active:hover,
.navbar-mobile .dropdown ul li:hover > a {
  color: #3ec1d5;
}

.navbar-mobile .dropdown > .dropdown-active {
  display: block;
}

/*--------------------------------------------------------------
# Back to top button
--------------------------------------------------------------*/
.back-to-top {
  position: fixed;
  visibility: hidden;
  opacity: 0;
  right: 15px;
  bottom: 15px;
  z-index: 996;
  background: #3ec1d5;
  width: 40px;
  height: 40px;
  transition: all 0.4s;
}

.back-to-top i {
  font-size: 28px;
  color: #fff;
  line-height: 0;
}

.back-to-top:hover {
  background: #60ccdc;
  color: #fff;
}

.back-to-top.active {
  visibility: visible;
  opacity: 1;
}

.clear {
  clear: both;
}

ul {
  list-style: outside none none;
  margin: 0;
  padding: 0;
}

input,
select,
textarea,
input[type="text"],
input[type="date"],
input[type="url"],
input[type="email"],
input[type="password"],
input[type="tel"],
button,
button[type="submit"] {
  -moz-appearance: none;
  box-shadow: none !important;
}

div#preloader {
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99999;
  width: 100%;
  height: 100%;
  overflow: visible;
  background: #fff url("./../assets/img/preloader.gif") no-repeat center center;
}

::-moz-selection {
  background: #3ec1d5;
  text-shadow: none;
}

::selection {
  background: #3ec1d5;
  text-shadow: none;
}

.area-padding {
  padding: 70px 0px 80px;
}

.area-padding-2 {
  padding: 70px 0px 50px;
}

.padding-2 {
  padding-bottom: 90px;
}

.section-headline h2 {
  display: inline-block;
  font-size: 40px;
  font-weight: 600;
  margin-bottom: 70px;
  position: relative;
  text-transform: capitalize;
}

.section-headline h2::after {
  border: 1px solid #333;
  bottom: -20px;
  content: "";
  left: 0;
  margin: 0 auto;
  position: absolute;
  right: 0;
  width: 40%;
}

.sec-head {
  display: inline-block;
  font-size: 17px;
  font-weight: 600;
  margin-bottom: 0;
  padding: 0 0 10px;
  text-transform: uppercase;
  transition: all 0.4s ease 0s;
}

/*--------------------------------------------------------------
# Hero Section
--------------------------------------------------------------*/
#hero {
  width: 100%;
  height: 100vh;
  background: #000;
  overflow: hidden;
  position: relative;
}

@media (max-height: 500px) {
  #hero {
    height: 150vh;
  }
}

#hero .carousel,
#hero .carousel-inner,
#hero .carousel-item,
#hero .carousel-item::before {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
}

#hero .carousel-item {
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

#hero .carousel-item::before {
  content: "";
  background-color: rgba(0, 0, 0, 0.7);
}

#hero .carousel-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  bottom: 0;
  top: 70px;
  left: 50px;
  right: 50px;
}

#hero .container {
  text-align: center;
}

#hero h2 {
  color: #fff;
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: 700;
}

@media (max-width: 768px) {
  #hero h2 {
    font-size: 20px;
    line-height: 1.2;
  }
}

#hero p {
  width: 80%;
  font-size: 38px;
  line-height: 1.2;
  margin: 0 auto 30px auto;
  color: #fff;
}

@media (min-width: 1024px) {
  #hero p {
    width: 60%;
  }
}

@media (max-width: 768px) {
  #hero p {
    font-size: 28px;
    line-height: 1.2;
  }
}

#hero .carousel-fade {
  overflow: hidden;
}

#hero .carousel-fade .carousel-inner .carousel-item {
  transition-property: opacity;
}

#hero .carousel-fade .carousel-inner .carousel-item,
#hero .carousel-fade .carousel-inner .active.carousel-item-start,
#hero .carousel-fade .carousel-inner .active.carousel-item-end {
  opacity: 0;
}

#hero .carousel-fade .carousel-inner .active,
#hero .carousel-fade .carousel-inner .carousel-item-next.carousel-item-start,
#hero .carousel-fade .carousel-inner .carousel-item-prev.carousel-item-end {
  opacity: 1;
  transition: 0.5s;
}

#hero .carousel-fade .carousel-inner .carousel-item-next,
#hero .carousel-fade .carousel-inner .carousel-item-prev,
#hero .carousel-fade .carousel-inner .active.carousel-item-start,
#hero .carousel-fade .carousel-inner .active.carousel-item-end {
  left: 0;
  transform: translate3d(0, 0, 0);
}

#hero .carousel-control-prev,
#hero .carousel-control-next {
  width: 10%;
}

@media (min-width: 1024px) {
  #hero .carousel-control-prev,
  #hero .carousel-control-next {
    width: 5%;
  }
}

#hero .carousel-control-next-icon,
#hero .carousel-control-prev-icon {
  background: none;
  font-size: 32px;
  line-height: 1;
}

#hero .carousel-indicators li {
  cursor: pointer;
}

#hero .btn-get-started {
  font-weight: 500;
  font-size: 16px;
  letter-spacing: 1px;
  display: inline-block;
  padding: 8px 32px;
  border-radius: 50px;
  transition: 0.5s;
  margin: 10px;
  color: #fff;
  background: #3ec1d5;
}

#hero .btn-get-started:hover {
  background: #fff;
  color: #3ec1d5;
}

/*--------------------------------------------------------------
# About
--------------------------------------------------------------*/
.about-area {
  background-color: #f9f9f9;
}

.single-well > a {
  display: block;
}

.single-well ul li {
  color: #444;
  display: block;
  padding: 5px 0;
}

.single-well ul li i {
  color: #3ec1d5;
  margin-right: 5px;
  font-size: 18px;
}

.single-well p {
  color: #444;
}

/*--------------------------------------------------------------
# Services
--------------------------------------------------------------*/
.services-icon {
  color: #444;
  display: inline-block;
  font-size: 36px;
  line-height: 36px;
  margin-bottom: 20px;
}

.section-headline.services-head > h2 {
  margin-bottom: 25px;
}

.services-details {
  padding-top: 40px;
  transition: all 0.5s ease 0s;
}

.services-details:hover h4,
.services-details:hover .services-icon {
  color: #3ec1d5;
}

.row.second-row {
  margin-top: 40px;
}

.section-head > h2 {
  color: #333;
}

.single-services > h4 {
  color: #444;
  font-size: 24px;
  font-weight: 500;
}

.single-services > p {
  color: #333;
  font-size: 14px;
}

/*--------------------------------------------------------------
# Team
--------------------------------------------------------------*/
.team-member {
  background: rgba(0, 0, 0, 0.65) none repeat scroll 0 0;
  display: block;
  margin-right: -15px;
  padding: 10px;
  position: relative;
  overflow: hidden;
}

.team-member::before {
  background: rgba(0, 0, 0, 0) url("../img/team/team01.jpg") repeat scroll 0 0;
  content: "";
  display: block;
  height: 100%;
  left: 0;
  margin-right: -15px;
  padding: 10px;
  position: absolute;
  top: 0;
  width: 100%;
  z-index: -1;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top center;
  transition: 5s;
  transform: scale(1);
}

.team-member:hover.team-member::before {
  transform: scale(1.2);
}

.single-team-member {
  border: 1px solid #ddd;
}

.team-left-text h4 {
  color: #fff;
  font-size: 30px;
  font-weight: 700;
  text-transform: uppercase;
}

.team-left-text p {
  color: #fff;
  font-size: 17px;
  line-height: 26px;
}

.email-news {
  display: block;
  margin: 30px 0;
  overflow: hidden;
  text-align: center;
  width: 100%;
}

.email-news .email_button input {
  background: rgba(0, 0, 0, 0) none repeat scroll 0 0;
  border: 1px solid #fff;
  color: #fff;
  float: left;
  font-size: 13px;
  padding: 8px;
  width: 81%;
}

.email-news .email_button > button {
  background: rgba(0, 0, 0, 0) none repeat scroll 0 0;
  border: 1px solid #fff;
  color: #fff;
  float: left;
  font-size: 16px;
  padding: 8px 12px;
  text-align: center;
}

.email-news .email_button > button:hover {
  background: #3ec1d5;
  border: 1px solid #fff;
  color: #fff;
}

.team-left-icon ul li {
  display: inline-block;
}

.team-left-icon ul li a:hover {
  color: #3ec1d5;
  background: #fff;
  border: 2px solid #fff;
}

.team-left-icon ul li a {
  border: 2px solid #fff;
  color: #fff;
  display: block;
  font-size: 16px;
  height: 40px;
  line-height: 37px;
  margin: 0 3px;
  width: 40px;
}

.team-member-carousel .single-team-member {
  overflow: hidden;
  width: 100%;
}

.single-team-member:hover .team-img a:after {
  opacity: 1;
}

.single-team-member:hover .team-social-icon {
  top: 45%;
  opacity: 1;
}

.team-img {
  position: relative;
}

.team-img > a {
  display: block;
}

.team-img > a::after {
  background: rgba(0, 0, 0, 0.7);
  bottom: 0;
  content: "";
  height: 100%;
  left: 0;
  position: absolute;
  transition: all 0.5s ease 0s;
  width: 100%;
  opacity: 0;
}

.team-social-icon {
  left: 50%;
  margin-left: -61px;
  opacity: 0;
  position: absolute;
  top: 30%;
  transition: 1.3s;
}

.team-social-icon ul li {
  display: inline-block;
}

.team-social-icon ul li a {
  border: 1px solid #fff;
  border-radius: 50%;
  color: #fff;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  height: 34px;
  width: 34px;
  margin: 0 3px;
}

.team-social-icon ul li a i {
  line-height: 0;
}

.team-social-icon ul li a:hover {
  color: #fff;
  border: 1px solid #3ec1d5;
  background: #3ec1d5;
}

.team-content {
  padding: 10px 0px;
}

.team-content > h4,
.team-content > p {
  color: #444;
  margin-bottom: 5px;
}

.team-content.head-team p {
  margin-bottom: 0;
}

.team-left-icon.text-center {
  margin-bottom: 20px;
}

.head-team h4 {
  display: inline-block;
  font-size: 25px;
  font-weight: 600;
  padding-bottom: 10px;
  text-transform: uppercase;
}

/*--------------------------------------------------------------
# Review
--------------------------------------------------------------*/
.reviews-area {
  background: url(../assets/img/background/reviews-bg.jpg);
  overflow: hidden;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top center;
  background-attachment: fixed;
  width: 100%;
  height: auto;
  position: relative;
}

.work-left-text {
  background: #3ec1d5 none repeat scroll 0 0;
}

.work-right-text {
  background: rgba(0, 0, 0, 0.8) none repeat scroll 0 0;
}

.work-right-text h2 {
  color: #fff;
  text-transform: uppercase;
  font-size: 24px;
}

.work-right-text h5 {
  color: #fff;
  font-size: 18px;
  font-weight: 700;
  line-height: 34px;
  text-transform: uppercase;
}

.work-right-text .sus-btn {
  margin-left: 0;
  margin-top: 20px;
}

.single-awesome-4 {
  display: block;
  float: left;
  overflow: hidden;
  width: 33.33%;
}

.single-awesome-4 .add-actions {
  padding: 10px 20px;
}

/*--------------------------------------------------------------
# Portfolio
--------------------------------------------------------------*/
.pst-content {
  padding-left: 10px;
}

#portfolio-flters {
  padding: 0;
  margin: 5px 0 35px 0;
  list-style: none;
  text-align: center;
}

#portfolio-flters li {
  cursor: pointer;
  margin: 15px 15px 15px 0;
  display: inline-block;
  padding: 8px 24px;
  font-size: 12px;
  line-height: 20px;
  color: #555;
  border-radius: 50px;
  text-transform: uppercase;
  background: #fff;
  margin-bottom: 5px;
  transition: all 0.3s ease-in-out;
  border: 1px solid #444;
}

#portfolio-flters li:hover,
#portfolio-flters li.filter-active {
  border-color: #3ec1d5;
  background: #3ec1d5;
  color: #fff;
}

#portfolio-flters li:last-child {
  margin-right: 0;
}

.single-awesome-portfolio {
  float: left;
  overflow: hidden;
  padding: 15px;
  width: 25%;
  position: relative;
}

.single-awesome-project {
  overflow: hidden;
  margin-bottom: 30px;
}

.first-item {
  margin-bottom: 30px;
}

.awesome-img {
  display: block;
  width: 100%;
  height: 100%;
  position: relative;
}

.awesome-img > a {
  display: block;
  position: relative;
}

.single-awesome-project:hover .awesome-img > a::after {
  opacity: 1;
}

.single-awesome-project:hover .add-actions {
  opacity: 1;
  bottom: 0;
}

.awesome-img > a::after {
  background: rgba(0, 0, 0, 0.7) none repeat scroll 0 0;
  content: "";
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
  opacity: 0;
  transition: 0.4s;
}

.add-actions {
  background: rgba(0, 0, 0, 0.6) none repeat scroll 0 0;
  bottom: 30px;
  display: block;
  height: 100%;
  left: 0;
  opacity: 0;
  overflow: hidden;
  padding: 10px 15px;
  position: absolute;
  transition: all 0.4s ease 0s;
  width: 100%;
}

.project-dec {
  display: block;
  height: 100%;
  width: 100%;
}

.project-dec a {
  display: block;
  height: 100%;
  width: 100%;
}

.project-dec h4 {
  margin-bottom: 5px;
}

.project-dec h4:hover {
  color: #fff;
}

.project-dec h4 {
  color: #ddd;
  font-size: 24px;
  margin-top: -45px;
  padding-top: 50%;
  text-decoration: none;
  text-transform: uppercase;
  font-weight: 800;
}

.project-dec span {
  color: #ddd;
  font-size: 13px;
}

.project-action-btn {
  display: block;
  height: 100%;
  text-align: center;
  transition: all 1s ease 0s;
  width: 100%;
}

.project-action-btn li {
  display: block;
  height: 100%;
  width: 100%;
}

.project-action-btn li a {
  display: block;
  height: 100%;
  width: 100%;
}

/*--------------------------------------------------------------
# Pricing
--------------------------------------------------------------*/
.pricing-area {
  background: rgba(0, 0, 0, 0.02) none repeat scroll 0 0;
}

.pri_table_list {
  border: 1px solid #ccc;
  text-align: center;
  transition: all 0.4s ease 0s;
  background: #fff;
}

.pri_table_list h3 span {
  font-size: 16px;
}

.pri_table_list ol li {
  border-bottom: 1px solid #ccc;
  color: #666;
  padding: 12px 15px;
  position: relative;
  display: flex;
  align-items: center;
}

.pri_table_list li.check {
  font-weight: 600;
}

.pri_table_list li.check i {
  color: #3ec1d5;
  line-height: 0;
  font-size: 24px;
  margin-right: 5px;
}

.pri_table_list li.cross {
  text-decoration: line-through;
  color: #999;
}

.pri_table_list li.cross i {
  color: #999;
  line-height: 0;
  font-size: 24px;
  margin-right: 5px;
}

.pri_table_list button {
  background: #444 none repeat scroll 0 0;
  border: 1px solid #444;
  color: #fff;
  margin-bottom: 25px;
  padding: 10px 35px;
  text-transform: uppercase;
  transition: all 0.4s ease 0s;
  border-radius: 30px;
}

.pri_table_list > h3 {
  color: #333;
  font-size: 24px;
  font-weight: 700;
  line-height: 25px;
  padding: 30px 0 20px;
  text-transform: uppercase;
  transition: all 0.4s ease 0s;
}

.pri_table_list ol {
  list-style: outside none none;
  margin: 0;
  padding: 0 0 25px;
}

.pri_table_list.active {
  transition: all 0.4s ease 0s;
  position: relative;
  overflow: hidden;
}

.saleon {
  background: #3ec1d5 none repeat scroll 0 0;
  color: #fff;
  font-size: 13px;
  font-weight: 700;
  left: -26px;
  padding: 2px 25px;
  position: absolute;
  text-transform: uppercase;
  top: 16px;
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
  -ms-transform: rotate(-45deg);
  -o-transform: rotate(-45deg);
  -moz-transform: rotate(-45deg);
}

.pri_table_list > button:hover {
  background-color: #fff;
  border: 1px solid #333;
  color: #333;
}

.active > h3 {
  background: #f5f5f5 none repeat scroll 0 0;
  color: #333;
  transition: all 0.4s ease 0s;
}

.active > button {
  background: #3ec1d5 none repeat scroll 0 0;
  border: 1px solid #3ec1d5;
  color: #fff;
  transition: 0.4s;
}

.active > button:hover {
  background: #333 none repeat scroll 0 0;
  border: 1px solid #333;
  color: #fff;
  transition: 0.4s;
}

/*--------------------------------------------------------------
# Testimonials
--------------------------------------------------------------*/
.testimonials {
  padding: 80px 0;
  background: url("../assets/img/background/slider-bg.jpg") no-repeat;
  background-position: center center;
  background-size: cover;
  position: relative;
}

.testimonials::before {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
}

.testimonials .testimonials-carousel,
.testimonials .testimonials-slider {
  overflow: hidden;
}

.testimonials .testimonial-item {
  text-align: center;
  color: #fff;
}

.testimonials .testimonial-item .testimonial-img {
  width: 100px;
  border-radius: 50%;
  border: 6px solid rgba(255, 255, 255, 0.15);
  margin: 0 auto;
}

.testimonials .testimonial-item h3 {
  font-size: 20px;
  font-weight: bold;
  margin: 10px 0 5px 0;
  color: #fff;
}

.testimonials .testimonial-item h4 {
  font-size: 14px;
  color: #ddd;
  margin: 0 0 15px 0;
}

.testimonials .testimonial-item .quote-icon-left,
.testimonials .testimonial-item .quote-icon-right {
  color: rgba(255, 255, 255, 0.4);
  font-size: 26px;
}

.testimonials .testimonial-item .quote-icon-left {
  display: inline-block;
  left: -5px;
  position: relative;
}

.testimonials .testimonial-item .quote-icon-right {
  display: inline-block;
  right: -5px;
  position: relative;
  top: 10px;
}

.testimonials .testimonial-item p {
  font-style: italic;
  margin: 0 auto 15px auto;
  color: #eee;
}

.testimonials .swiper-pagination {
  margin-top: 20px;
  position: relative;
}

.testimonials .swiper-pagination .swiper-pagination-bullet {
  width: 12px;
  height: 12px;
  background-color: rgba(255, 255, 255, 0.5);
  opacity: 1;
}

.testimonials .swiper-pagination .swiper-pagination-bullet-active {
  background-color: #3ec1d5;
}

@media (min-width: 992px) {
  .testimonials .testimonial-item p {
    width: 80%;
  }
}

/*--------------------------------------------------------------
# Quote
--------------------------------------------------------------*/
.suscribe-area {
  background: #3ec1d5 none repeat scroll 0 0;
  padding: 30px 0;
}

.suscribe-text {
  display: block;
  padding: 10px 0;
}

.suscribe-text h3 {
  color: #fff;
  display: inline-block;
  font-size: 20px;
  font-weight: 600;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.sus-btn {
  background: #fff none repeat scroll 0 0;
  border: 2px solid #fff;
  color: #3ec1d5;
  display: inline-block;
  font-size: 16px;
  font-weight: 700;
  margin-left: 100px;
  padding: 10px 20px;
  text-decoration: none;
  text-transform: uppercase;
  border-radius: 30px;
}

.sus-btn:hover {
  background: #3ec1d5 none repeat scroll 0 0;
  border: 2px solid #fff;
  color: #fff;
}

/*--------------------------------------------------------------
# Contact
--------------------------------------------------------------*/
.contact-area {
  height: auto;
  width: 100%;
}

.contact-content {
  padding: 100px;
  background: #000 none repeat scroll 0 0;
}

.contact-content-right {
  padding: 100px;
}

.single-icon i {
  font-size: 32px;
  width: 50px;
  height: 50px;
  line-height: 56px;
  border-radius: 50%;
  margin-bottom: 30px;
  color: #3ec1d5;
}

.single-icon p {
  font-size: 16px;
  line-height: 30px;
}

.contact-icon {
  margin-bottom: 40px;
}

#google-map {
  height: 370px;
  margin-bottom: 20px;
}

.php-email-form .validate {
  display: none;
  color: red;
  margin: 0;
  font-weight: 400;
  font-size: 13px;
}

.php-email-form .error-message {
  display: none;
  color: #fff;
  background: #ed3c0d;
  text-align: center;
  padding: 15px;
  font-weight: 600;
}

.php-email-form .sent-message {
  display: none;
  color: #fff;
  background: #18d26e;
  text-align: center;
  padding: 15px;
  font-weight: 600;
}

.php-email-form .loading {
  display: none;
  background: #fff;
  text-align: center;
  padding: 15px;
}

.php-email-form .loading:before {
  content: "";
  display: inline-block;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  margin: 0 10px -6px 0;
  border: 3px solid #18d26e;
  border-top-color: #eee;
  animation: animate-loading 1s linear infinite;
}

.php-email-form input,
.php-email-form textarea {
  border-radius: 0;
  box-shadow: none;
  font-size: 14px;
}

.php-email-form input:focus,
.php-email-form textarea:focus {
  border-color: #3ec1d5;
}

.php-email-form input {
  padding: 10px 15px;
}

.php-email-form textarea {
  padding: 12px 15px;
}

.php-email-form button[type="submit"] {
  background: rgba(0, 0, 0, 0) none repeat scroll 0 0;
  border: 1px solid #ccc;
  color: #444;
  font-size: 16px;
  font-weight: 700;
  margin-top: 8px;
  padding: 12px 30px;
  text-transform: uppercase;
  transition: all 0.3s ease 0s;
  border-radius: 30px;
}

@keyframes animate-loading {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

/*--------------------------------------------------------------
# Responsive
--------------------------------------------------------------*/
@media (min-width: 1920px) {
  .work-right-text {
    padding: 150px 150px;
  }
}

/* Normal desktop :992px. */
@media (min-width: 992px) and (max-width: 1169px) {
  .slider-content {
    padding: 146px 0;
  }

  .work-right-text {
    padding: 40px 0;
  }

  .work-right-text h2 {
    font-size: 18px;
    line-height: 28px;
  }
}

/* Tablet desktop :768px. */
@media (min-width: 768px) and (max-width: 991px) {
  .layer-1-1 h2 {
    font-size: 24px;
  }

  .layer-1-2 h1 {
    font-size: 31px;
    line-height: 38px;
    padding: 0px 30px;
  }

  .tab-menu ul.nav li a {
    padding: 10px 16px;
  }

  .suscribe-input input {
    width: 60%;
  }

  .suscribe-input button {
    width: 40%;
  }

  .team-content.text-center > h4 {
    font-size: 20px;
  }

  .sus-btn {
    margin-left: 0;
  }

  .suscribe-text h3 {
    font-size: 16px;
    padding-right: 20px;
  }

  .work-right-text h5 {
    font-size: 14px;
    line-height: 22px;
  }

  .work-right-text {
    padding: 36px 0;
  }

  .work-right-text h2 {
    font-size: 14px;
    line-height: 22px;
  }

  .work-right-text .ready-btn {
    font-size: 13px;
    padding: 7px 20px;
    margin-top: 5px;
  }

  .single-awesome-portfolio {
    width: 33.33%;
  }

  .widget-product a img {
    display: block;
    float: none;
    width: 100%;
  }

  .widget-product .product-info {
    display: block;
    float: none;
    padding-left: 0;
    width: 100%;
    margin-top: 20px;
  }

  .map-column {
    margin-left: 0;
    padding-right: 40px;
  }

  .post-information .entry-meta {
    font-size: 13px;
    padding: 5px 0;
  }

  .post-information .entry-meta span a {
    padding: 4px 0;
  }

  .service-pic {
    margin-bottom: 30px;
    text-align: center;
  }

  .single-add-itms {
    width: 50%;
  }

  .left-sidebar-title > h4 {
    font-size: 18px;
  }

  .contact-form {
    margin-top: 0px;
  }

  .search-option input {
    width: 67%;
  }
}

/* small mobile :320px. */
@media (max-width: 767px) {
  .slider-area {
    margin-top: 60px;
  }

  .slider-content {
    padding: 80px 0;
  }

  .slider-content h2 {
    font-size: 18px !important;
    line-height: 24px !important;
  }

  .slider-content h1 {
    font-size: 20px !important;
    line-height: 26px !important;
  }

  .layer-1-3 a.ready-btn {
    padding: 8px 15px;
  }

  .section-headline h2 {
    font-size: 30px;
  }

  .well-middle .single-well {
    margin-top: 30px;
  }

  .single-skill {
    margin-bottom: 40px;
  }

  .tab-menu {
    margin-top: 30px;
  }

  .tab-menu ul.nav li a {
    padding: 8px 6px;
  }

  .wellcome-text {
    margin: 0px;
    padding: 70px 0px;
  }

  .subs-feilds {
    width: 100%;
  }

  .suscribe-input input {
    width: 60%;
  }

  .suscribe-input button {
    font-size: 15px;
    padding: 14px 10px;
    width: 40%;
  }

  .section-headline h3 {
    font-size: 25px;
  }

  .well-text > h2 {
    font-size: 18px;
  }

  .well-text p {
    display: none;
  }

  .single-team-member {
    margin-bottom: 30px;
  }

  .service-right {
    width: 100%;
  }

  .service-images:hover .overly-text {
    display: none;
  }

  .portfolio-area {
    padding-top: 0px;
  }

  .project-menu li a {
    padding: 8px 12px;
    margin: 10px 4px;
  }

  .pri_table_list {
    margin-bottom: 30px;
  }

  .single-awesome-project,
  .portfolio-2 .single-awesome-project {
    width: 100%;
    float: none;
  }

  .single-blog {
    margin-bottom: 30px;
  }

  .sus-btn {
    margin-left: 0;
    margin-top: 30px;
  }

  .contact-form {
    margin-top: 30px;
  }

  .head-team h5 {
    font-size: 22px;
  }

  .footer-content {
    margin-bottom: 30px;
  }

  .header-bottom h1 {
    font-size: 30px;
    margin-bottom: 0;
  }

  .page-area .slider-content {
    padding: 500px 0;
  }

  .search-option input {
    width: 74%;
  }

  .header-bottom h2 {
    font-size: 20px;
    margin-bottom: 0;
  }

  li.threaded-comments {
    margin-left: 0;
  }
}

/* Large Mobile :480px. */
@media only screen and (min-width: 480px) and (max-width: 767px) {
  .submitbtn {
    float: none;
    width: 99.8%;
  }

  .icons-bottom ul li a {
    height: 40px;
    line-height: 37px;
    width: 40px;
  }

  .blog-post-dlc ul li {
    padding-left: 20px;
    padding-right: 20px;
  }

  .awesome-portfolio-content .portfolio-2 {
    width: 50%;
  }

  .gallary-details .single-awesome-portfolio {
    width: 50%;
  }

  .tab-menu ul.nav li a {
    padding: 8px 20px;
  }
}

@media (max-width: 575px) {
  .slider-content {
    padding: 0;
  }
}

.hidden {
  display: none;
}
</style>
