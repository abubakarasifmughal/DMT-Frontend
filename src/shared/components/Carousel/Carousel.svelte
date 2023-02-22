<script>
  export let slides = [
    {
      image: "/assets/static/images/global/galleria_umberto.png",
      rating: 4.9,
      review: 10,
      label: "A beautiful light filled shopping arcade",
      category: "Shopping & Explore",
    },
    {
      image: "/assets/static/images/global/cappella.png",
      rating: 2.7,
      review: 120,
      label: "Get connected and experience the customs",
      category: "Heritage",
    },
    {
      image: "/assets/static/images/global/castel_ovo.png",
      rating: 1.7,
      review: 98,
      label: "Seaside view",
      category: "Nature",
    },
  ];

  let cards = [
    ...slides,
    // ------------------
    {
      image: "",
      rating: null,
      review: null,
      label: "",
      category: "",
    },
    {
      image: null,
      rating: null,
      review: null,
      label: "",
      category: "",
    },
  ];
  let currentCardIndex = 0;
</script>

<div>
  <div class="carousel ">
    <div class="row pt-5 pb-5" style="flex-wrap: nowrap;overflow: hidden;">
      <div class="col-md-5">
        <div
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5 rounded"
          style="background-image: url({cards[currentCardIndex].image});"
        >
          {#if cards[currentCardIndex].image === ""}
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
        <div>
          <div class="p-2">
            <div>
              <span class="text-secondary"
                ><b>{cards[currentCardIndex].rating}</b> ({cards[
                  currentCardIndex
                ].review})</span
              >
            </div>
            <div class="text-black">{cards[currentCardIndex].label}</div>
            <div class="text-secondary">
              {cards[currentCardIndex].category}
            </div>
          </div>
        </div>
        <div>
          <button
            class="mt-4 me-3 roundBtn btn"
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
            class="mt-4 roundBtn btn"
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
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5"
          style="background-image: url({cards[currentCardIndex + 1].image});"
        >
          {#if cards[currentCardIndex + 1].image === ""}
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
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5"
          style="background-image: url({cards[currentCardIndex + 2].image});"
        >
          {#if cards[currentCardIndex + 2].image === ""}
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
  .card-slide {
    transition: 0.5s;
    height: 350pt;
    background-size: cover;
    background-position: center;
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
