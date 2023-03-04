<script>
  import { createEventDispatcher } from "svelte";
  import { each } from "svelte/internal";
  import {
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
  } from "sveltestrap";
  import Map from "../lib/MapBox/Map.svelte";
  import Mapbox from "../lib/MapBox/Mapbox.svelte";
  export let onClickBack;
  export let onClickNext;
  export let locationAddress = "";
  export let coordX;
  export let coordY;
  let disp = createEventDispatcher();

  const allRegions = [
    { name: "Asia", enabled: true },
    { name: "Africa", enabled: true },
    { name: "Europe", enabled: false },
    { name: "America", enabled: false },
    { name: "Chine", enabled: false },
  ];
  let selectedRegion = "";
</script>

<div class="bg-white col-xl-11 p-5 shadow">
  <h3>Enter location address</h3>
  <hr />
  <br />
  <Dropdown>
    <DropdownToggle class="btn btn-light px-4 mb-4 border">
      {selectedRegion === "" ? "Select Region" : selectedRegion}
    </DropdownToggle>
    <DropdownMenu>
      {#each allRegions as region}
        <DropdownItem
          on:click={() => {
            if (region.enabled) {
              selectedRegion = region.name;
            } else {
              alert("Listing is available only within Africa and Asia");
            }
          }}
        >
          {region.name}
        </DropdownItem>
      {/each}
    </DropdownMenu>
  </Dropdown>
  <textarea
    class="form-control"
    rows="5"
    bind:value={locationAddress}
    on:change={() => {
      disp("AddressChanged", {
        Address: locationAddress,
      });
    }}
  />
  <div class="mt-5">
    <!-- <div class="mb-2">If needed, drag the map pin to adust it’s location.</div> -->
    <div class="map">
      <Map {coordX} {coordY} />
      <!-- <Mapbox /> -->
    </div>
  </div>
  <div class="mt-3 d-flex justify-content-between">
    <button on:click={onClickBack} class="btn pe-5 ps-5">Back</button>
    <button on:click={onClickNext} class="btn pe-5 ps-5">Next</button>
  </div>
</div>

<style>
  /* .purpleText {
    color: #9427f7;
    cursor: pointer;
  } */
  .map {
    height: 50vh;
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
