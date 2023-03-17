<script>
  import { createEventDispatcher } from "svelte";

  let dispatch = createEventDispatcher();

  export let title = "property";
  export let amenitiesList = [];
  export let selectedAmeneties = [];

  $: isPresent = (id) => selectedAmeneties.findIndex((a) => a === id) !== -1;
</script>

<div class="col-md-12">
  <h3>What amenities does your {title} have?</h3>
  <span>
    We recommend having at least five of these top amenities. You’ll be able to
    add other amenities after you publish your listing.
  </span>
  <hr />
  <br />

  {#each amenitiesList as filter}
    <div class="mb-5">
      <h3>{filter.label}</h3>
      <span>{filter.description}</span>
      {#each filter.options as option}
        <div>
          <div class="mt-2 ">
            <button
              class="btn btn-light border col-12 text-start"
              style={(isPresent(option.id)
                ? "background-color: #9427F7;color: white;padding-left:10%;"
                : "") + "transition:0.5s"}
              on:click={() => {
                let index = selectedAmeneties.findIndex((a) => a === option.id);
                if (index === -1) {
                  selectedAmeneties.push(option.id);
                } else {
                  selectedAmeneties.splice(index, 1);
                }
                selectedAmeneties = selectedAmeneties;
                dispatch("amenities", {
                  amenities: selectedAmeneties,
                });
              }}
            >
              {option.label}
            </button>
          </div>
        </div>
      {/each}
      <hr />
    </div>
  {/each}

  <br />
  <br />
</div>

<style>
</style>
