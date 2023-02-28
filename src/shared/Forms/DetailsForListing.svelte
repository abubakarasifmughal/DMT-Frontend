<script>
  import { createEventDispatcher } from "svelte";

  let disp = createEventDispatcher();
  export let onClickBack;
  export let onClickNext;

  export let headline = "";
  export let description = "";
</script>

<div class="bg-white shadow p-5 col-md-11">
  <h3>Enter Listings Name</h3>
  <span
    >This shows up in searches and at the top of your listing. Try to write a
    brief line that hints at the type of space and where it is. Example:
    Romantic Spanish villa w/hot tub — 5 min from the beach!</span
  >
  <hr />
  <br />
  <div class="col-md-6 mb-3">
    <input
      type="text"
      class="form-control"
      placeholder="Headline"
      bind:value={headline}
      on:change={(data) => {
        disp("headline", {
          headline: headline,
        });
      }}
    />
  </div>
  <hr />
  <h4>Enter Description</h4>
  <span>
    Tell guests more about your space in your own words. Try to help them
    imagine what a stay might be like at your property.
  </span>
  <br />
  <textarea
    class="form-control mt-3"
    rows="10"
    placeholder="Description"
    bind:value={description}
    on:change={(data) => {
      disp("description", {
        description: description,
      });
    }}
  />
  <div class="mt-3 d-flex justify-content-between">
    <button on:click={onClickBack} class="btn pe-5 ps-5">Back</button>
    <button on:click={onClickNext} class="btn pe-5 ps-5">Next</button>
  </div>
</div>

<style>
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
