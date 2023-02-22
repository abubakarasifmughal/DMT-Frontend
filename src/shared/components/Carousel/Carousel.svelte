<script>
  export let slides = [
    {
      image: "/assets/static/images/global/galleria_umberto.png",
      text:"Accommodation",
      rating: 4.9,
      review: 10,
      label: "A beautiful light filled shopping arcade",
      address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepal",
    },
    {
      image: "/assets/static/images/global/cappella.png",
      text:"Online Experience ",
      rating: 2.7,
      review: 120,
      label: "Get connected and experience the customs",
      address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepa",
    },
    {
      image: "/assets/static/images/global/castel_ovo.png",
      text:"Onsite Experience ",
      rating: 1.7,
      review: 98,
      label: "Seaside view",
      address: "Chaksibari Marg, Kathmandu 44600, Kathmandu, Nepa",
    },
  ];

  let cards = [
    ...slides,
    // ------------------
    {
      image: "",
      text:"",
      rating: null,
      review: null,
      label: "",
      category: "",
    },
    {
      image: null,
      text:"",
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
      <div class="col-md-5 pe-5 pe-md-0">
        <div
          class="card-slide me-3 me-md-0 d-flex text-center p-4 align-items-center justify-content-end flex-column rounded"
          style="background-image: url({cards[currentCardIndex].image});"
        >
          <div class="bg-white w-100 rounded p-3 text-start">
            <div>
              <div>
                <span class="text-secondary1"
                  ><div><b>{cards[currentCardIndex].text}</b></div><div class="rat"><div><b>{cards[currentCardIndex].rating}</b></div><div> <b>({cards[
                    currentCardIndex
                  ].review})</b></div></div></span
                >
              </div>
              <div>
               
                
              </div>
              <div class="label">
              <div class="text-black">{cards[currentCardIndex].label}</div>
            </div>
              <div class="text-secondary">
                <i class="bi bi-geo-alt-fill"></i> {cards[currentCardIndex].address}
              </div>
            </div>
          </div>
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
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5 rounded"
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
          class="card-slide d-flex text-center align-items-center justify-content-center flex-column p-5 rounded"
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
  .text-secondary1{
    color:rgb(144, 141, 141);
    display: flex;
    font-size: 15px;
  }
  .text-secondary{
    font-size: 10px;
  }
  .rat{
    font-size: 15px;
  }
  .label{
    margin:10px 0px;
  }
  .rat{
    color:rgb(144, 141, 141);
    display:flex;
    padding-left: 5px;
  }
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
