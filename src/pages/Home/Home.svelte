<script lang="ts">
  import CarouselCustom from "../../shared/components/Carousel/Carousel.svelte";
  import Finder from "../../shared/components/Finder/Finder.svelte";
  import Footer from "../../shared/components/Footer/Footer.svelte";
  import GetApp from "../../shared/components/GetApp/GetApp.svelte";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import {
    Carousel,
    CarouselControl,
    CarouselIndicators,
    CarouselItem,
    CarouselCaption,
  } from "sveltestrap";

  const items = [
    {
      url: "/assets/static/images/index/nepal_sunset.jpg",
    },
    {
      url: "/assets/static/images/index/napel_clouds_mountian.jpg",
    },
  ];
  let activeIndex = 0;
</script>

<div>
  <div class="home_top_section bg-danger">
    <div
      style="position: absolute; top: 0pt; width: 100%; z-index: 100;"
      class="bg-light"
    >
      <Navbar btnTheme={"btn-outline-light"} />
    </div>

    <Carousel
      {items}
      bind:activeIndex
      ride
      interval={2000}
      style="background-color: gainsboro;height: 70vh; width: 100%;"
    >
      <CarouselIndicators bind:activeIndex {items} />
      <div class="carousel-inner" style="height: 100%;">
        {#each items as item, index}
          <CarouselItem bind:activeIndex itemIndex={index}>
            <img
              src={item.url}
              alt=""
              style="object-position: center;object-fit: cover;height: 70vh;width: 100%;"
            />
          </CarouselItem>
        {/each}
      </div>

      <CarouselControl direction="prev" bind:activeIndex {items} />
      <CarouselControl direction="next" bind:activeIndex {items} />
    </Carousel>
  </div>
  <div style="position: relative;">
    <Finder />
  </div>
  <div class="container-fluid">
    <div class="row ">
      <div
        class="col-lg-5 d-flex flex-column align-items-center align-items-sm-end justify-content-center"
      >
        <div class="fw-bolder p-5" style="font-size: 30pt;line-height: 35pt;">
          <b> Unforgettable activities to do in Naples </b>
        </div>
      </div>
      <div class="col-lg-6">
        <CarouselCustom />
      </div>
    </div>
  </div>

  <!-- <div class="mt-5 pt-5">
    <div class="container ">
      <div class="row d-flex justify-content-center">
        <div class="col-lg-3 mt-5 text-center text-lg-start">
          <img
            src="/assets/static/icon/coffe_icon.svg"
            height="25pt"
            alt="peace"
            class="mb-2"
          />
          <div class="fw-bold" style="font-size: 15pt;">Peace of mind</div>
          <div>Our Book with Confidence guarantee gives you 24/7 support.</div>
        </div>
        <div class="col-lg-3 mt-5 text-center text-lg-start">
          <img
            src="/assets/static/icon/money_icon.svg"
            height="25pt"
            alt="peace"
            class="mb-2"
          />
          <div class="fw-bold" style="font-size: 15pt;">More for less</div>
          <div>More space, more privacy, more amenities — more value.</div>
        </div>
        <div class="col-lg-3 mt-5 text-center text-lg-start">
          <img
            src="/assets/static/icon/lock_icon.svg"
            height="25pt"
            alt="peace"
            class="mb-2"
          />
          <div class="fw-bold" style="font-size: 15pt;">Quality at core</div>
          <div>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit ut aliquam,
            purus sit
          </div>
        </div>
      </div>
    </div>
  </div> -->
  <br />
  <div class="container mt-5 p-3 p-sm-5">
    <div class="row mb-5">
      <div class="col-lg-1" />
      <div class="col-lg-5 mb-5">
        <div
          class="card text-white p-5"
          style="background-image: url('/assets/static/images/index/cave_explore.jpg');height: 400pt"
        >
          <h1 class="fw-bolder">Things to do from home</h1>
          <div class="mt-3">
            <button class="btn btn-light px-5 fw-bold">Experiences</button>
          </div>
        </div>
      </div>
      <div class="col-lg-5 mb-5">
        <div
          class="card text-white p-5"
          style="background-image: url('/assets/static/images/index/online_expierence.png');height: 400pt;"
        >
          <h1 class="fw-bolder">Places to Stay In Nepal</h1>
          <div class="mt-3">
            <button class="btn btn-light px-5 fw-bold">Stays</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div>
    <GetApp />
  </div>
  <br />
  <div
    class="mt-2 p-5 text-center 
    d-flex justify-content-center align-items-center flex-column"
  >
    <h1 class="fw-bolder mb-5 text-dark">Offers and Discount</h1>
    <div class="row col-md-5">
      <div class="col-lg-8 mb-4">
        <input
          type="text"
          class="form-control col-12 px-2 py-3"
          placeholder="Email"
        />
      </div>
      <div class="col-lg-4">
        <button class="btn col-12 px-2 py-3 fw-bold">Submit</button>
      </div>
    </div>
  </div>
  <div class="low_backgrounded">
    <Footer />
  </div>
</div>

<style>
  .card {
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }
  .low_backgrounded {
    background-color: whitesmoke;
    padding-top: 20pt;
    /* background-image: url("/assets/static/images/global/ocean_footer.jpg"); */
    /* background-position-x: center;
    background-position-y: center;
    background-repeat: no-repeat;
    background-size: cover;
    position: relative; */
  }

  .home_top_section {
    /* background-image: url("/assets/static/images/dev/front_page-dw.jpg"); */
    /* background-image: url("/assets/static/images/index/nepal_sunset.jpg");
    background-size: cover; */
    /* background-attachment: fixed; */
    /* background-position-x: center;
    background-position-y: center; */
    position: relative;
    display: flex;
    align-items: flex-start;
  }

  .btn {
    border: 1px solid #9427f7;
    background-color: #9427f7;
    color: white;
  }
  .btn:first-child:hover {
    background-color: #6801c8;
    color: white;
  }

  .form-control {
    border-color: #9427f7;
  }
</style>
