<!-- svelte-ignore a11y-click-events-have-key-events -->
<script>
  import { Modal } from "sveltestrap";
  import LoginForm from "./LoginForm.svelte";
  import config from "../../../environment.json";

  export let listing;
  export let user_id;

  let AllReviews = [];

  let loading = false;
  user_id;
  let review = "";
  let stars = 0;

  async function onSubmitReview() {
    loading = true;
    let headersList = {
      Accept: "*/*",
      "Content-Type": "application/json",
    };

    let bodyContent = JSON.stringify({
      listing: { id: listing?.id },
      user: { id: parseInt(user_id) },
      review: review,
      rating: stars,
    });
    let response = await fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/reviews`,
      {
        method: "POST",
        body: bodyContent,
        headers: headersList,
      }
    );

    let data = await response.json();
    fetchReviewsOfListing();
    loading = false;
  }

  function onPressWriteReview() {
    if (review === "") {
      error = true;
      errorMessage = "Please write something about the hotel first.";
    } else {
      onSubmitReview();
    }
  }

  let error = undefined;
  let errorMessage = "";

  let openSignIn = false;

  async function fetchReviewsOfListing() {
    let headersList = {
      Accept: "*/*",
    };

    let response = await fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/reviews/6/${listing?.id}`,
      {
        method: "GET",
        headers: headersList,
      }
    );

    let data = await response.json();
    AllReviews = data;
    console.log(AllReviews);
  }

  fetchReviewsOfListing();
</script>

{#if user_id !== null}
  <div>
    {#if loading}
      <div class="p-5 d-flex justify-content-center">
        <div class="spinner-border" />
      </div>
    {:else}
      <h2>Write a Review</h2>
      <br />
      <textarea
        on:keypress={(e) => {
          if (e.key === "Enter") {
            onPressWriteReview();
          }
        }}
        bind:value={review}
        class="form-control"
        rows="6"
        placeholder={"Write a review for " + listing?.headline}
      />
      <br />
      <span>How would you rate this place</span>
      <div class="d-flex mt-2">
        {#each [1, 2, 3, 4, 5] as rating, index}
          <div
            on:click={() => (stars = stars === 1 ? 0 : index + 1)}
            class="ms-1 me-2"
          >
            {#if index < stars}
              <div class="scaled bi bi-star-fill" />
            {:else}
              <div class="scaled bi bi-star" />
            {/if}
          </div>
        {/each}
      </div>
      <div class="text-start mt-3">
        <button class="btn px-4" on:click={onPressWriteReview}
          >Add Review</button
        >
      </div>
    {/if}
  </div>
{:else}
  <div>
    <button class="px-4 btn" on:click={() => (openSignIn = true)}>
      Sign in to review
    </button>
  </div>
{/if}
<br />
<hr />
<br />
<!-- Reviews -->
<div>
  <div class="row">
    {#each AllReviews as reviewItem}
      <div class="col-md-6 mb-4 ">
        <div class="border col-12 p-2 rounded h-100 shadow-sm">
          <div class="d-flex py-2 h-100">
            <div class="h-100 ">
              <div
                class="rounded-circle border bg-secondary text-white p-3 fw-bold"
              >
                {reviewItem.first_name.split(
                  " "
                )[0][0]}{reviewItem.last_name.split(" ")[0][0]}
              </div>
            </div>
            <div
              style="width: 100%;"
              class="ps-3 h-100 d-flex flex-column justify-content-between"
            >
              <div style="font-size: large;">{reviewItem.review}</div>

              <div>
                <div class="text-end">Rated ({reviewItem.rating}/5)</div>
                <div class="fw-bold text-secondary opacity-50 text-end">
                  {reviewItem.first_name.split(" ")[0]}
                  {reviewItem.last_name.split(" ")[0]}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>

<Modal isOpen={error === true} centered>
  <div class="modal-header">
    <span><b>Error</b></span>
    <button class="btn btn-close" on:click={() => (error = undefined)} />
  </div>
  <div class="modal-body">
    {errorMessage}
  </div>
</Modal>

<Modal isOpen={openSignIn} centered size="xl">
  <div class="modal-header">
    <span><b>Sign In</b></span>
    <button class="btn btn-close" on:click={() => (openSignIn = false)} />
  </div>
  <div class="modal-body">
    <LoginForm redirect={false} />
  </div>
</Modal>

<style>
  .scaled {
    transform: scale(1.4);
    color: goldenrod;
  }
</style>
