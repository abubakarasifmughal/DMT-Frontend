<script>
  import {
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
  } from "sveltestrap";
  import config from "../../../../environment.json";
  import Amenities from "../../../shared/Forms/Amenities.svelte";
  import AmenitiesPanel from "../../../shared/Forms/AmenitiesPanel.svelte";
  export let facilityItems = [];

  let amenitiesList = [];

  let label = "";
  let name = "";
  let visibleTo = "Room";
  // {
  //   "label": "Facility P",
  //   "facilityItems": [
  //       {
  //           "name": "S F1",
  //           "visibleTo": "Listing"
  //       },
  //       {
  //           "name": "S F2",
  //           "visibleTo": "Room"
  //       }
  //   ]
  // }

  const addFacilities = async () => {
    let headersList = {
      Accept: "*/*",
      "User-Agent": "Thunder Client (https://www.thunderclient.com)",
      "Content-Type": "application/json",
    };

    let bodyContent = JSON.stringify({
      label: label,
      facilityItems: facilityItems,
    });
    {
    }
    let response = await fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/facilities`,
      {
        method: "POST",
        body: bodyContent,
        headers: headersList,
      }
    );

    let data = await response.text();
    loadData();
    facilityItems = [];
  };

  function loadData() {
    let headersList = {
      Accept: "*/*",
      "User-Agent": "Thunder Client (https://www.thunderclient.com)",
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/facilities`, {
      method: "GET",
      headers: headersList,
    }).then((data) =>
      data.json().then((data) => {
        console.log(data);
        amenitiesList = data;
      })
    );
  }
  loadData();
</script>

<div class="p-3">
  <br />
  <div class="row">
    <div class="col-lg-12">
      <div class="d-flex justify-content-between">
        <h2>Manage Amenities here</h2>
      </div>
      <br />
      <div style="">
        <div class="container-fluid card">
          <div class="row">
            <div class="col-lg-7">
              <div class="p-3">
                <div style="font-size: 2vh;" class="py-3">
                  Manage Facilities
                </div>
                <input
                  type="text"
                  placeholder="Enter facility Name"
                  class="form-control"
                  bind:value={label}
                />

                <hr />
                <h4 class="text-center">Add Aspects</h4>
                <div class="row">
                  <div class="col-md-6 mb-3">
                    <input
                      bind:value={name}
                      type="text"
                      class="form-control"
                      placeholder="Aspect label"
                    />
                  </div>
                  <div class="col-md-3 mb-3">
                    <Dropdown>
                      <DropdownToggle class="col-12">{visibleTo}</DropdownToggle
                      >
                      <DropdownMenu>
                        <DropdownItem on:click={() => (visibleTo = "Listing")}
                          >Listings</DropdownItem
                        >
                        <DropdownItem on:click={() => (visibleTo = "Room")}
                          >Rooms</DropdownItem
                        >
                      </DropdownMenu>
                    </Dropdown>
                  </div>
                  <div class="col-md-3 mb-3">
                    <button
                      class="btn btn-primary col-12"
                      on:click={() => {
                        facilityItems = [
                          { name: name, visibleTo: visibleTo },
                          ...facilityItems,
                        ];
                        name = "";
                        visibleTo = "Room";
                      }}
                    >
                      Add
                    </button>
                  </div>
                </div>
                <br />
                <button
                  class="btn btn-light border my-3 col-12"
                  on:click={addFacilities}
                >
                  Add to Systen
                </button>
                <hr />
                <h4 class="text-center">Your new Facility</h4>
                <div>
                  <div class="text-center">
                    Label: {label}
                  </div>
                  {#each facilityItems as item, i}
                    <div class="my-2 input-group">
                      <button
                        class="btn bordered btn-outline-danger w-25"
                        on:click={() => {
                          facilityItems.splice(i, 1);
                          facilityItems = facilityItems;
                        }}
                      >
                        Remove
                      </button>
                      <button class="btn border btn-light w-75" disabled>
                        - {item.name} for {item.visibleTo}
                      </button>
                    </div>
                  {/each}
                  <div />
                </div>
              </div>
              <br />
            </div>
            <div class="col-lg-5 mt-2 pt-3">
              <div style="font-size: 2vh;">Amenities</div>
              {#if !amenitiesList.length}
                <AmenitiesPanel {amenitiesList} />
              {:else}
                <div
                  class="h-100 d-flex flex-column justify-content-center align-items-center"
                >
                  <div class="spinner spinner-border" />
                  <div class="mt-2">Loading...</div>
                </div>
              {/if}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
