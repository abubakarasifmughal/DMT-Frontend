<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import { Offcanvas, Tooltip } from "sveltestrap";
  import Amenities from "./Amenities.svelte";

  let dispatch = createEventDispatcher();
  export let room;
  export let currency = "USD";
  export let index = 0;
  export let onClickRemove;

  let isOpen = false;
  $: amenetiesLength = room.ameneties.length;

  const toggle = () => {
    isOpen = !isOpen;
  };

  let AllAmeneties = [
    {
      label: "Property",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Smoking Area",
          checked: false,
          id: 1,
        },
        {
          label: "Shops",
          checked: false,
          id: 2,
        },
        {
          label: "Wedding Facilities",
          checked: false,
          id: 3,
        },
        {
          label: "Library",
          checked: false,
          id: 4,
        },
        {
          label: "Gym",
          checked: false,
          id: 5,
        },
        {
          label: "Spa",
          checked: false,
          id: 6,
        },
        {
          label: "Sauna",
          checked: false,
          id: 7,
        },
        {
          label: "Massage",
          checked: false,
          id: 8,
        },
        {
          label: "Poolside bar",
          checked: false,
          id: 9,
        },
      ],
    },
    {
      label: "Facilities",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Executive floot",
          checked: false,
          id: 10,
        },
        {
          label: "Business center",
          checked: false,
          id: 11,
        },
        {
          label: "Meetings Facilities",
          checked: false,
          id: 12,
        },
        {
          label: "Executive Lounge",
          checked: false,
          id: 13,
        },
      ],
    },
    {
      label: "Food and Drinks",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Cafe",
          checked: false,
          id: 14,
        },
        {
          label: "Restaurant",
          checked: false,
          id: 15,
        },
        {
          label: "Bar",
          checked: false,
          id: 16,
        },
        {
          label: "Room Service",
          checked: false,
          id: 17,
        },
      ],
    },
    {
      label: "Room Ameneties",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Air Conditioning",
          checked: false,
          id: 18,
        },
        {
          label: "Alarm Clock",
          checked: false,
          id: 19,
        },
        {
          label: "Bathrobe",
          checked: false,
          id: 20,
        },
        {
          label: "Bathroom Amenities",
          checked: false,
          id: 21,
        },
        {
          label: "Bathtub",
          checked: false,
          id: 22,
        },
        {
          label: "Hair Dryer",
          checked: false,
          id: 23,
        },
        {
          label: "Hotel Key Cards",
          checked: false,
          id: 24,
        },
        {
          label: "In-Room Safe",
          checked: false,
          id: 25,
        },
        {
          label: "Minibar",
          checked: false,
          id: 26,
        },
        {
          label: "Slippers",
          checked: false,
          id: 27,
        },
        {
          label: "Smart TV",
          checked: false,
          id: 28,
        },
        {
          label: "Smoke Detector",
          checked: false,
          id: 29,
        },
        {
          label: "Sofa Couch",
          checked: false,
          id: 30,
        },
        {
          label: "Stationary Kit",
          checked: false,
          id: 31,
        },
        {
          label: "Study Table",
          checked: false,
          id: 32,
        },
        {
          label: "Telephone",
          checked: false,
          id: 33,
        },
        {
          label: "Tea/Coffee Maker",
          checked: false,
          id: 34,
        },
        {
          label: "Tea/Coffee Sachet",
          checked: false,
          id: 35,
        },
        {
          label: "Towel",
          checked: false,
          id: 36,
        },
        {
          label: "Wardrobe",
          checked: false,
          id: 37,
        },
        {
          label: "WIFI",
          checked: false,
          id: 38,
        },
      ],
    },
  ];

  let discountAvailable = false;
</script>

<div class="mt-4">
  <div class="row">
    <div class="col-md-6">
      <div class="d-flex align-item-center justify-content-between">
        <h4>Category {room.RoomCategory}</h4>
        <button
          style="color:#9427F7;cursor: pointer;background:none;border: none;"
          on:click={onClickRemove}
        >
          Remove
        </button>
      </div>
      <input
        type="text"
        class="form-control mt-2"
        placeholder="Category Name"
        bind:value={room.RoomCategory}
      />
      <textarea
        class="form-control mt-2"
        placeholder="Category Highlights"
        rows="5"
        bind:value={room.RoomDescription}
      />
      <div class="row">
        <label class="mt-3" for="">Number of Rooms</label>
        <div class="col-md-8 mt-2">
          <input
            type="number"
            min="1"
            class="form-control"
            placeholder="Quantity"
            disabled
            bind:value={room.Quantity}
          />
        </div>
        <div class="col-md-4 mt-2">
          <div class="d-flex">
            <button
              class="btn btn-light border"
              style="width: 45%;"
              on:click={() => {
                {
                  dispatch("onRoomQtyChanged", {
                    qty: room.Quantity,
                  });
                  room.Quantity =
                    room.Quantity !== 1 ? room.Quantity - 1 : room.Quantity;
                }
              }}>-</button
            >
            <div style="width: 10%;" />
            <button
              class="btn btn-light border"
              style="width: 45%;"
              on:click={() => {
                room.Quantity = room.Quantity + 1;
                dispatch("onRoomQtyChanged", {
                  qty: room.Quantity,
                });
              }}>+</button
            >
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-8 mt-2">
          <label class="mb-2" for="">Max Occupancy (per room)</label>
          <div class="d-flex flex-wrap">
            <input
              type="number"
              class="form-control"
              placeholder="Discounted Price"
              bind:value={room.MaxOccupancy}
            />
          </div>
          <div class="col-md-4" />
        </div>
      </div>

      <div class="row mt-2">
        <b>Breakfast</b>
        <div class="col-md-8">
          <div class="d-flex flex-wrap align-items-center m-2">
            <input
              type="radio"
              name="breakfast"
              class="m-2"
              style="transform: scale(1.2);"
            />
            <div>Included</div>
          </div>
          <div class="d-flex flex-wrap align-items-top m-2">
            <input
              type="radio"
              name="breakfast"
              class="m-2"
              style="transform: scale(1.2);"
            />
            <div>Extra (Price - {currency} 7)</div>
          </div>
          <div class="col-md-4" />
        </div>
      </div>

      <div class="row mt-2">
        <b>Transfer of service</b>
        <div class="col-md-8">
          <div class="d-flex flex-wrap align-items-center m-2">
            <input
              type="radio"
              name="breakfast"
              class="m-2"
              style="transform: scale(1.2);"
            />
            <div>Included</div>
          </div>
          <div class="d-flex flex-wrap align-items-top m-2">
            <input
              type="radio"
              name="breakfast"
              class="m-2"
              style="transform: scale(1.2);"
            />
            <div>Extra (Price - {currency} 20)</div>
          </div>
          <div class="col-md-4" />
        </div>
      </div>
      <div class="row">
        <div class="col-md-8 mt-2">
          <label class="mb-2" for="">Price per night ({currency})</label>
          <div class="d-flex flex-wrap">
            <input
              type="number"
              class="form-control"
              placeholder="Cost per night"
              bind:value={room.Cost}
            />
          </div>
        </div>
      </div>

      <div class="row mt-2">
        <b>Discount Available</b>
        <div class="col-md-8">
          <div class="d-flex flex-wrap align-items-center m-2">
            <input
              bind:group={discountAvailable}
              type="radio"
              name="discount"
              class="m-2"
              value={true}
              style="transform: scale(1.2);"
            />
            <div>Yes</div>
          </div>
          <div class="d-flex flex-wrap align-items-top m-2">
            <input
              bind:group={discountAvailable}
              type="radio"
              name="discount"
              class="m-2"
              value={false}
              style="transform: scale(1.2);"
            />
            <div>No</div>
          </div>
          <div class="col-md-4" />
        </div>
      </div>
      {#if discountAvailable}
        <div class="row">
          <div class="col-md-8 mt-2">
            <label class="mb-2" for="">Discounted Price</label>
            <div class="d-flex flex-wrap">
              <input
                type="number"
                class="form-control"
                placeholder="Max Occupancy"
                bind:value={room.DiscountedPrice}
              />
            </div>
            <div class="col-md-4" />
          </div>
        </div>
      {/if}

      <button class="btn mt-3 btn-light border" on:click={toggle}>
        {amenetiesLength <= 0 ? "Add Ameneties" : "Ameneties Added"}
      </button>
    </div>
    <div class="col-md-6">
      <div class="col-12 p-5 dragInRegion text-center">
        <div>Select photos for your room</div>
        <input
          style="visibility: hidden;position: absolute;"
          type="file"
          bind:files={room.meta}
          on:change={() => {
            for (
              let index = 0;
              index < room.meta.length && index < 5;
              index++
            ) {
              const element = room.meta[index];
              let reader = new FileReader();
              reader.addEventListener("load", () => {
                room.Images = [{ address: reader.result }];
              });
              reader.readAsDataURL(element);
            }
          }}
          id="{room.RoomCategory}{index}_room"
        />
        <label
          class="btn btn-light border mt-4"
          for="{room.RoomCategory}{index}_room"
        >
          Add Photos
        </label>
      </div>

      <div class="d-flex flex-wrap">
        {#each room.Images as image, imind}
          <div class="btn-group me-3 mt-3" style="overflow: hidden;">
            <button
              class="btn btn-light border"
              on:click={() => {
                room.Images.splice(imind, 1);
                room.Images = room.Images;
              }}
            >
              <div class="btn btn-close" />
            </button>
            <div
              id="abc"
              class="photo border-top border-end border-bottom"
              style="overflow: hidden;background-image: url({image.address})"
            />
          </div>
        {/each}
      </div>
    </div>
  </div>
</div>

<Offcanvas
  {isOpen}
  {toggle}
  on:close={() => {
    amenetiesLength = room.ameneties.length;
  }}
>
  <Amenities
    selectedAmeneties={room.ameneties}
    amenitiesList={AllAmeneties}
    on:ameneties={(data) => {
      room.ameneties = data.detail.ameneties;
    }}
  />
</Offcanvas>

<style>
  .dragInRegion {
    border: 1.5px dashed gainsboro;
  }

  .photo {
    height: 50pt;
    width: 50pt;
    background-color: gainsboro;
    border-top-right-radius: 5pt;
    border-bottom-right-radius: 5pt;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
  }
</style>
