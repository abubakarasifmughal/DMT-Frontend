<script>
  import { createEventDispatcher } from "svelte";

  let event = createEventDispatcher();

  export let property;
  $: notPropert = !property;

  export let onClickContinue;

  let page = 1;
</script>

<div class="mt-3">
  <div class="row">
    <div class="col-md-10">
      {#if page === 1}
        <h2>Greetings! from Digital Marketplace for Tourism branded as DMT</h2>
        <span>
          We welcome you to list your Hotels and Experience offering based in
          Africa and Asia to the world
        </span>
        <div>May we all prosper</div>
        <br />
        <button class="btn px-4" on:click={() => (page += 1)}>
          Lets get Started
        </button>
      {:else}
        <h2>Is it a Property or a Service?</h2>
        <br />
        <div class="mt-2" style="font-size: large;">
          <div class="d-flex align-items-center">
            <input
              on:click={() => {
                property = true;
                event("isPropery", {
                  isProperty: property,
                });
              }}
              bind:checked={property}
              class="mt-3 mb-3 me-3"
              id="welcom_property_checkbox"
              type="checkbox"
            />
            <label for="welcom_property_checkbox">Property</label>
          </div>
          <div class="d-flex align-items-center">
            <input
              on:click={() => {
                property = false;
                event("isPropery", {
                  isProperty: property,
                });
              }}
              bind:checked={notPropert}
              class="mt-3 mb-3 me-3"
              id="welcom_service_checkbox"
              type="checkbox"
            />
            <label for="welcom_service_checkbox">Service</label>
          </div>
          <button on:click={onClickContinue} class="mt-3 btn pe-5 ps-5">
            Continue
          </button>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  input[type="checkbox"] {
    transform: scale(1.3);
    accent-color: #9427f7;
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
