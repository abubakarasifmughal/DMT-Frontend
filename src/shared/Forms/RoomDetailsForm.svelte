<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import { Offcanvas, Tooltip } from "sveltestrap";
  import Amenities from "./Amenities.svelte";
  import config from "../../../environment.json";

  let dispatch = createEventDispatcher();
  export let room;
  export let currency = "USD";
  export let index = 0;
  export let onClickRemove;
  export let isProperty = true;
  export let isOnsite = true;

  let isOpen = false;
  $: amenetiesLength = room.amenities.length;

  const toggle = () => {
    isOpen = !isOpen;
  };

  let AllAmeneties = [];

  const getFacilities = async () => {
    let headersList = {
      Accept: "*/*",
      "User-Agent": "Thunder Client (https://www.thunderclient.com)",
    };

    let response = await fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/facilities`,
      {
        method: "GET",
        headers: headersList,
      }
    );

    let data = await response.json();
    AllAmeneties = data;
  };

  getFacilities().then((v) => {});

  let discountAvailable = false;
  let breakfastNotIncluded = false;
  let transferNotIncluded = false;
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
        <div class="col-md-12 mt-2">
          <label class="mb-2" for="">Max Occupancy (per room)</label>
          <div class="d-flex flex-wrap">
            <input
              type="number"
              class="form-control"
              placeholder="Maximum Guests"
              bind:value={room.MaxOccupancy}
            />
          </div>
          <div class="col-md-4" />
        </div>
      </div>

      <div class="row mt-2">
        <b>Breakfast</b>
        <div class="col-md-12">
          <div class="d-flex flex-wrap align-items-center m-2">
            <input
              bind:group={breakfastNotIncluded}
              type="radio"
              name="breakfast"
              class="m-2"
              value={false}
              style="transform: scale(1.2);"
            />
            <div>Included</div>
          </div>
          <div class="d-flex flex-wrap align-items-top m-2">
            <input
              bind:group={breakfastNotIncluded}
              type="radio"
              name="breakfast"
              class="m-2"
              value={true}
              style="transform: scale(1.2);"
            />
            <div>Extra</div>
          </div>
          {#if breakfastNotIncluded}
            <div class="row">
              <div class="col-md-12 my-2">
                <div class="d-flex flex-wrap">
                  <input
                    type="number"
                    class="form-control col-12"
                    placeholder="Breakfast price"
                    bind:value={room.BreakfastPrice}
                  />
                </div>
                <div class="col-md-4" />
              </div>
            </div>
          {/if}
          <div class="col-md-4" />
        </div>
      </div>

      <div class="row mt-2">
        <b>Transfer of service</b>
        <div class="col-md-12">
          <div class="d-flex flex-wrap align-items-center m-2">
            <input
              bind:group={transferNotIncluded}
              type="radio"
              name="transfer"
              class="m-2"
              value={false}
              style="transform: scale(1.2);"
            />
            <div>Included</div>
          </div>
          <div class="d-flex flex-wrap align-items-top m-2">
            <input
              bind:group={transferNotIncluded}
              type="radio"
              name="transfer"
              class="m-2"
              value={true}
              style="transform: scale(1.2);"
            />
            <div>Extra</div>
          </div>
          {#if transferNotIncluded}
            <div class="row">
              <div class="col-md-12 my-2">
                <div class="d-flex flex-wrap">
                  <input
                    type="number"
                    class="form-control col-12"
                    placeholder="Transfer price"
                    bind:value={room.TransferOfServicePrice}
                  />
                </div>
                <div class="col-md-4" />
              </div>
            </div>
          {/if}
          <div class="col-md-4" />
        </div>
      </div>
      <div class="row">
        <div class="col-md-12 mt-2">
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
        <div class="col-md-12">
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
          <div class="col-md-12 my-2">
            <label class="mb-2" for="">Price After Discount</label>
            <div class="d-flex flex-wrap">
              <input
                type="number"
                class="form-control"
                placeholder="Price After Discount"
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
    amenetiesLength = room.amenities.length;
  }}
>
  <Amenities
    title={"room"}
    selectedAmeneties={room.amenities}
    amenitiesList={AllAmeneties}
    on:amenities={(data) => {
      room.amenities = data.detail.amenities;
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
