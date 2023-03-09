<script>
  export let slides = [
    {
      image: "/assets/static/images/global/Accommodation.jpg",
      text: "Accommodation",
      rating: 4.9,
      review: 10,
      label: "A cozy gem overlooking Bwindi Impenetrable National Park",
      address: "Bugambira, Uganda, Africa",
    },
    {
      image: "/assets/static/images/global/Swayambhunath.jpg",
      text: "Online Experience",
      rating: 4.9,
      review: 10,
      label: "Guided Virtual Tour around Swayambhunath",
      address: "Live from Nepal, Asia",
    },
    {
      image: "/assets/static/images/global/Bartender.jpg",
      text: "Online Experience",
      rating: 4.9,
      review: 10,
      label: "Learn to make your special shot with the top Bartender",
      address: "Live from Uganda, Africa",
    },
    {
      image: "/assets/static/images/global/Bhaktapur.jpg",
      text: "Onsite Experience",
      rating: 4.9,
      review: 10,
      label: "Guided World Heritage Tour around Bhaktapur",
      address: "Bhaktapur, Nepal, Asia",
    },
    // {
    //   image: "/assets/static/images/global/galleria_umberto.png",
    //   text: "Accommodation",
    //   rating: 4.9,
    //   review: 10,
    //   label: "A beautiful light filled shopping arcade",
    //   address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepal",
    // },
    // {
    //   image: "/assets/static/images/global/cappella.png",
    //   text: "Online Experience ",
    //   rating: 2.7,
    //   review: 120,
    //   label: "Get connected and experience the customs",
    //   address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepa",
    // },
    // {
    //   image: "/assets/static/images/global/castel_ovo.png",
    //   text: "Onsite Experience ",
    //   rating: 1.7,
    //   review: 98,
    //   label: "Seaside view",
    //   address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepa",
    // },
  ];

  let cards = [
    ...slides,
    // ------------------
    {
      image: "",
      text: "",
      rating: null,
      review: null,
      label: "",
      category: "",
    },
    {
      image: null,
      text: "",
      rating: null,
      review: null,
      label: "",
      category: "",
    },
  ];
  let currentCardIndex = 0;
</script>

<div class="">
  <div class="carousel ">
    <div class="row pt-5 pb-5" style="flex-wrap: nowrap;overflow: hidden;">
      <div class="col-md-5  ms-sm-0 ms-md-4">
        <div
          class="card-slide d-flex text-center p-3 align-items-center justify-content-end flex-column rounded m-md-0 m-3"
          style="background-image: url({cards[currentCardIndex].image});"
        >
          <!-- <div class="bg-white w-100 rounded p-3 text-start">
            <div>
              <div class="d-flex flex-row align-items-top">
                <div
                  style="height: 12pt;width: 12pt;transform:translateY(-1pt);margin-right:2pt"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    aria-hidden="true"
                    role="img"
                    width="100%"
                    height="100%"
                    preserveAspectRatio="xMidYMid meet"
                    viewBox="0 0 24 24"
                    ><path
                      fill="#9c59df"
                      stroke="#9c59df"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M11.074 2.633c.32-.844 1.531-.844 1.852 0l2.07 5.734a.99.99 0 0 0 .926.633h5.087c.94 0 1.35 1.17.611 1.743L18 14a.968.968 0 0 0-.322 1.092L19 20.695c.322.9-.72 1.673-1.508 1.119l-4.917-3.12a1 1 0 0 0-1.15 0l-4.917 3.12c-.787.554-1.83-.22-1.508-1.119l1.322-5.603A.968.968 0 0 0 6 14l-3.62-3.257C1.64 10.17 2.052 9 2.99 9h5.087a.989.989 0 0 0 .926-.633l2.07-5.734Z"
                    /></svg
                  >
                </div>
                <span class="text-secondary"
                  ><b class="text-dark">{cards[currentCardIndex].rating}</b>
                  ({cards[currentCardIndex].review})</span
                >
              </div>

              <div class="text-black fw-bold mt-1" style="line-height: 13pt;">
                {cards[currentCardIndex].label}
              </div>
              <div class="text-secondary fw-bold mt-1">
                {cards[currentCardIndex].category}
              </div>
            </div>
          </div> -->
          <div class="bg-white w-100 rounded p-3 text-start">
            <div>
              <div>
                <span class="text-secondary1 justify-content-between"
                  ><div>
                    <!-- <svg
                      xmlns="http://www.w3.org/2000/svg"
                      aria-hidden="true"
                      role="img"
                      width="100%"
                      height="100%"
                      preserveAspectRatio="xMidYMid meet"
                      viewBox="0 0 24 24"
                      ><path
                        fill="#9c59df"
                        stroke="#9c59df"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M11.074 2.633c.32-.844 1.531-.844 1.852 0l2.07 5.734a.99.99 0 0 0 .926.633h5.087c.94 0 1.35 1.17.611 1.743L18 14a.968.968 0 0 0-.322 1.092L19 20.695c.322.9-.72 1.673-1.508 1.119l-4.917-3.12a1 1 0 0 0-1.15 0l-4.917 3.12c-.787.554-1.83-.22-1.508-1.119l1.322-5.603A.968.968 0 0 0 6 14l-3.62-3.257C1.64 10.17 2.052 9 2.99 9h5.087a.989.989 0 0 0 .926-.633l2.07-5.734Z"
                      /></svg
                    > -->
                    <b>{cards[currentCardIndex].text}</b>
                  </div>
                  <div class="rat">
                    <div><b>{cards[currentCardIndex].rating}</b></div>
                    <div><b>({cards[currentCardIndex].review})</b></div>
                  </div></span
                >
              </div>
              <div />
              <div class="label">
                <div class="text-black">{cards[currentCardIndex].label}</div>
              </div>
              <div class="text-secondary">
                <i class="bi bi-geo-alt-fill" />
                {cards[currentCardIndex].address}
              </div>
            </div>
          </div>
          {#if cards[currentCardIndex].image === "" && false}
            <h4>Search for Location</h4>
            <span>
              Lorem ipsum, dolor sit amet consectetur adipisicing elit.
              Consectetur, nemo. Nihil, non saepe ratione aliquid libero
              impedit! Id explicabo in officia? Nesciunt consequatur
              consequuntur esse ducimus atque a numquam eligendi.
            </span>
            <button class="btn ps-4 pe-4 mt-4"> Exlore </button>
          {/if}
        </div>

        <div class="d-flex justify-content-end p-3">
          <button
            class="me-3 roundBtn btn"
            on:click={() =>
              currentCardIndex === 0
                ? currentCardIndex
                : (currentCardIndex = currentCardIndex - 1)}
          >
            <img
              src="/assets/static/icon/arrow.png"
              style="object-fit: contain;transform: rotate(0deg);object-position: center;"
              alt=""
              height="60%"
              width="60%"
            />
          </button>
          <button
            class="roundBtn btn"
            on:click={() =>
              currentCardIndex + 3 === cards.length
                ? currentCardIndex
                : (currentCardIndex = currentCardIndex + 1)}
          >
            <img
              src="/assets/static/icon/arrow.png"
              style="object-fit: contain;transform: rotate(180deg);object-position: center;"
              alt=""
              height="60%"
              width="60%"
            />
          </button>
        </div>
      </div>
      <div class="col-md-5">
        <div
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5 rounded"
          style="background-image: url({cards[currentCardIndex + 1].image});"
        >
          {#if cards[currentCardIndex + 1].image === "" && false}
            <h4>Search for Location</h4>
            <span>
              Lorem ipsum, dolor sit amet consectetur adipisicing elit.
              Consectetur, nemo. Nihil, non saepe ratione aliquid libero
              impedit! Id explicabo in officia? Nesciunt consequatur
              consequuntur esse ducimus atque a numquam eligendi.
            </span>
            <button class="btn ps-4 pe-4 mt-4"> Exlore </button>
          {/if}
        </div>
      </div>
      <div class="col-md-5">
        <div
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5 rounded"
          style="background-image: url({cards[currentCardIndex + 2].image});"
        >
          {#if cards[currentCardIndex + 2].image === "" && false}
            <h4>Search for Location</h4>
            <span>
              Lorem ipsum, dolor sit amet consectetur adipisicing elit.
              Consectetur, nemo. Nihil, non saepe ratione aliquid libero
              impedit! Id explicabo in officia? Nesciunt consequatur
              consequuntur esse ducimus atque a numquam eligendi.
            </span>
            <button class="btn ps-4 pe-4 mt-4"> Exlore </button>
          {/if}
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .text-secondary1 {
    color: rgb(144, 141, 141);
    display: flex;
    font-size: 15px;
  }
  .text-secondary {
    font-size: 10px;
  }

  .label {
    margin: 10px 0px;
  }
  .rat {
    color: rgb(144, 141, 141);
    display: flex;
  }
  .card-slide {
    transition: 0.5s;
    height: 60vh;
    background-size: cover;
    background-position-x: center;
    background-position-y: top;
    background-repeat: no-repeat;
  }
  .roundBtn {
    height: 30pt;
    width: 30pt;
    border-radius: 15pt;
    transition: 0.5s;
  }
  .roundBtn:hover {
    height: 30pt;
    width: 30pt;
    border-radius: 15pt;
    transition: 0.5s;
    border-color: #6801c8;
  }

  .btn {
    border: 1px solid #9427f7;
    background-color: #9427f7;
    color: white;
  }
  .btn:nth-child(n):hover {
    background-color: #6801c8;
    color: white;
  }
</style>
