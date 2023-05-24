<script>
  import { createEventDispatcher } from "svelte";

  let dispatch = createEventDispatcher();

  export let title = "property";
  export let amenitiesList = [];
  export let selectedAmeneties = [];

  $: isPresent = (id) => selectedAmeneties.findIndex((a) => a === id) !== -1;
</script>

<div class="col-md-12">
  <hr />
  <br />
  {#each amenitiesList as filter}
    <div class="mb-5">
      <h3>{filter.label}</h3>
      {#each filter.facilityItems as option}
        <div>
          <div class="mt-2">
            <button
              disabled
              class="btn btn-light border col-12 text-start"
              style={(isPresent(option.id)
                ? "background-color: #9427F7;color: white;padding-left:10%;"
                : "") + "transition:0.5s"}
            >
              {option.name}
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
