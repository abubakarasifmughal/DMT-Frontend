<!-- svelte-ignore a11y-click-events-have-key-events -->
<script>
  import { Modal } from "sveltestrap";
  import LoginForm from "./LoginForm.svelte";
  import config from '../../../environment.json'

  export let listing;
  export let user_id;
  user_id;
  let review = "";
  let stars = 0;

  function onSubmitReview() {}

  let error = undefined;
  let errorMessage = "";

  let openSignIn = false;

  async function fetchReviewsOfListing() {
    let headersList = {
      Accept: "*/*",
      "User-Agent": "Thunder Client (https://www.thunderclient.com)",
    };

    let response = await fetch(`${config.SERVER_IP}${config.SERVER_PORT}/reviews/3/5`, {
      method: "GET",
      headers: headersList,
    });

    let data = await response.json();
    console.log(data);
  }

  fetchReviewsOfListing();
</script>

{#if user_id !== null}
  <div>
    <h2>Write a Review</h2>
    <br />
    <textarea
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
      <button
        class="btn px-4"
        on:click={() => {
          if (review === "") {
            error = true;
            errorMessage = "Please write something about the hotel first.";
          } else {
          }
        }}>Add Review</button
      >
    </div>
  </div>
{:else}
  <div>
    <button class="px-4 btn" on:click={() => (openSignIn = true)}>
      Sign in to review
    </button>
  </div>
{/if}

<!-- Reviews -->
<div />

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
