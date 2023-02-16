<script lang="ts">
  import Carousel from "../../shared/components/Carousel/Carousel.svelte";
  import Finder from "../../shared/components/Finder/Finder.svelte";
  import Footer from "../../shared/components/Footer/Footer.svelte";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
</script>

<div>
  <div class="home_top_section">
    <div style="position: absolute;top: 0pt;width: 100%;">
      <Navbar btnTheme={"btn-outline-light"} />
    </div>
    <div class="text-white container">
      <div
        style="font-size: 55pt;line-height: 60pt;text-shadow: 0px 0px 20px gray;"
      >
        It's time to <br />
        <span style="font-weight: bolder;">
          <b>Discover</b>
        </span>
      </div>
      <br />
      <span style="font-size: 20pt;text-shadow: 0px 0px 10px gray;">
        Find and book great expierences in Nepal
      </span>
    </div>
  </div>
  <div style="position: relative;">
    <Finder />
    <div style="height:100pt" />
  </div>
  <div class="container-fluid">
    <div class="row mt-5">
      <div class="col-lg-3" />
      <div
        class="col-lg-3 d-flex flex-column align-items-center align-items-sm-end justify-content-center"
      >
        <div
          class="fw-bolder text-md-end text-center p-5"
          style="font-size: 30pt;line-height: 35pt;"
        >
          <b> Unforgettable activities to do in Naples </b>
        </div>
      </div>
      <div class="col-lg-6">
        <Carousel />
      </div>
    </div>
  </div>
  <br />
  <div class="mt-5 pt-5">
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
  </div>
  <br />
  <div class="container mt-5 p-5">
    <div class="row mb-5">
      <div class="col-md-1" />
      <div class="col-md-5 mb-5">
        <div
          class="card text-white p-5"
          style="background-image: url('/assets/static/images/index/cave_explore.jpg');height: 400pt"
        >
          <h1 class="fw-bolder">Things to do from home</h1>
          <div class="mt-3">
            <button class="btn btn-light ps-4 pe-4 fw-bold">Experiences</button>
          </div>
        </div>
      </div>
      <div class="col-md-5 mb-5">
        <div
          class="card text-white p-5"
          style="background-image: url('/assets/static/images/index/online_expierence.png');height: 400pt;"
        >
          <h1 class="fw-bolder">Places to Stay In Nepal</h1>
          <div class="mt-3">
            <button class="btn btn-light ps-4 pe-4 fw-bold">Stays</button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div
    class="mt-2 p-5 text-center 
    d-flex justify-content-center align-items-center flex-column"
  >
    <h1 class="fw-bolder mb-5 text-dark">Keep in touch</h1>
    <div class="row col-md-4">
      <div class="col-md-9 mb-4">
        <input
          type="text"
          class="form-control col-12 p-3"
          placeholder="Email"
        />
      </div>
      <div class="col-md-3">
        <button class="btn col-12 p-3 fw-bold">Submit</button>
      </div>
    </div>
  </div>
  <div class="low_backgrounded">
    <div
      style="height: 200pt;background: linear-gradient(white,transparent);width:100%"
    />
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
    background-image: url("/assets/static/images/global/ocean_footer.jpg");
    background-position-x: center;
    background-position-y: center;
    background-repeat: no-repeat;
    background-size: cover;
    position: relative;
  }

  .home_top_section {
    min-height: 700pt;
    /* background-image: url("/assets/static/images/dev/front_page-dw.jpg"); */
    background-image: url("/assets/static/images/index/nepal_sunset.jpg");
    background-size: cover;
    /* background-attachment: fixed; */
    background-position-x: center;
    background-position-y: bottom;
    position: relative;
    display: flex;
    align-items: center;
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
