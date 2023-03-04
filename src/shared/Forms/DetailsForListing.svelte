<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import { Offcanvas } from "sveltestrap";
  import Amenities from "./Amenities.svelte";

  let disp = createEventDispatcher();
  export let onClickBack;
  export let onClickNext;

  export let headline = "";
  export let description = "";

  let isOpen = false;

  const toggle = () => {
    isOpen = !isOpen;
  };

  let selectedAmeneties: any = [];

  let AllAmeneties = [
    {
      label: "Public Area Facilities provided by the property",
      description: ``,
      options: [
        {
          label: "24 Hour Reception",
          checked: false,
          id: 1,
        },
        {
          label: "Bar",
          checked: false,
          id: 2,
        },
        {
          label: "Cafe",
          checked: false,
          id: 3,
        },
        {
          label: "Childcare Centre",
          checked: false,
          id: 4,
        },
        {
          label: "Concierge Desk",
          checked: false,
          id: 5,
        },
        {
          label: "Conference Room",
          checked: false,
          id: 6,
        },
        {
          label: "Elevator",
          checked: false,
          id: 7,
        },
        {
          label: "Health Club",
          checked: false,
          id: 8,
        },
        {
          label: "In-house Doctor",
          checked: false,
          id: 9,
        },
        {
          label: "Laundry",
          checked: false,
          id: 10,
        },
        {
          label: "Lounge Area",
          checked: false,
          id: 11,
        },
        {
          label: "Luggage Room",
          checked: false,
          id: 12,
        },
        {
          label: "Onsite Healthcare",
          checked: false,
          id: 13,
        },
        {
          label: "Parking Area",
          checked: false,
          id: 14,
        },
        {
          label: "Public Restroom",
          checked: false,
          id: 15,
        },
        {
          label: "Restaurant",
          checked: false,
          id: 16,
        },
        {
          label: "Room Service",
          checked: false,
          id: 17,
        },
        {
          label: "Sauna",
          checked: false,
          id: 18,
        },
        {
          label: "Shops",
          checked: false,
          id: 19,
        },
        {
          label: "Spa",
          checked: false,
          id: 20,
        },
        {
          label: "Swimming Pool",
          checked: false,
          id: 21,
        },
        {
          label: "Travel Desk",
          checked: false,
          id: 22,
        },
      ],
    },
  ];
</script>

<div class="bg-white shadow p-5 col-md-11">
  <h3>Enter Listings Name</h3>
  <span>
    This shows up in searches and at the top of your listing. Try to write a
    brief line that hints at the type of space and where it is. Example:
    Romantic Spanish villa w/hot tub — 5 min from the beach!
  </span>
  <hr />
  <br />
  <div class="row">
    <div class="col-md-6 mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Enter your Listing Name"
        bind:value={headline}
        on:change={(data) => {
          disp("headline", {
            headline: headline,
          });
        }}
      />
    </div>
    <div class="col-md-6 mb-3">
      <button class="btn btn-light border" on:click={toggle}>
        Add Amenities
      </button>
    </div>
  </div>
  <hr />
  <h4>Listing Hightlights</h4>
  <span>
    Tell guests more about your space in your own words. Try to help them
    imagine what a stay might be like at your property.
  </span>
  <br />
  <textarea
    class="form-control mt-3"
    rows="10"
    placeholder="Enter your listing's highlights"
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

  <Offcanvas {isOpen} {toggle}>
    <Amenities
      {selectedAmeneties}
      amenitiesList={AllAmeneties}
      on:ameneties={(data) => {
        // Save the data from the amenities to any variable
        selectedAmeneties = data.detail.ameneties;
      }}
    />
  </Offcanvas>
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
