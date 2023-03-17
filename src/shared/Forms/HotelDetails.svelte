<script>
  import { createEventDispatcher } from "svelte";
  import {
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
  } from "sveltestrap";
  import RoomDetailsForm from "./RoomDetailsForm.svelte";
  let dispatch = createEventDispatcher();
  let dispatch2 = createEventDispatcher();
  const types = ["Hotel", "Resort", "Motel", "Camping"];
  const currencies = ["USD", "PKR", "INR", "Euro", "AUD", "NPR"];
  export let currency = "";
  export let AccomodationType = "";
  function getFiltered(searchString) {
    return types.filter((a) =>
      a.toLowerCase().includes(searchString.toLowerCase())
    );
  }
  function getFilteredCurrencies(currencyText) {
    return currencies.filter((a) =>
      a.toLowerCase().includes(currencyText.toLowerCase())
    );
  }

  export let onClickBack;
  export let onClickNext;
  export let rooms = [];

  function addNewRoom() {
    rooms = [
      ...rooms,
      {
        RoomCategory: "",
        RoomDescription: "",
        Quantity: 1,
        Cost: 1,
        DiscountedPrice: 0,
        Images: [],
        meta: [],
        amenities: [],
        Visible: true,
      },
    ];
  }

  $: getTotalRooms = () => {
    return (rooms.length === 0 ? [{ Quantity: 0 }, { Quantity: 0 }] : rooms)
      .map((a) => a.Quantity)
      .reduce((a, b) => a + b);
  };
</script>

<div class="bg-white shadow p-5 col-md-10" style="overflow-x: hidden;">
  <h3>Please add details about Room Categories</h3>
  <br />
  <span>Let guests know more about the service.</span>
  <br />
  <br />
  <br />
  <div class="d-flex justify-content-between">
    <Dropdown>
      <DropdownToggle class="btn btn-light btn-sm">
        <input
          bind:value={AccomodationType}
          on:change={() => {
            dispatch("AccomodationType", {
              AccomodationType: AccomodationType,
            });
          }}
          type="text"
          class="form-control"
          placeholder="Accomodation Type"
        />
      </DropdownToggle>
      <DropdownMenu>
        {#each getFiltered(AccomodationType) as type}
          <DropdownItem
            on:click={() => {
              AccomodationType = type;
              dispatch("AccomodationType", {
                AccomodationType: AccomodationType,
              });
            }}>{type}</DropdownItem
          >
        {/each}
      </DropdownMenu>
    </Dropdown>

    <Dropdown>
      <DropdownToggle class="btn btn-light btn-sm">
        <input
          bind:value={currency}
          on:change={() => {
            dispatch2("currency", {
              currency: currency,
            });
          }}
          type="text"
          class="form-control"
          placeholder="Currency"
        />
      </DropdownToggle>
      <DropdownMenu>
        {#each getFilteredCurrencies(currency) as currencyItem}
          <DropdownItem
            on:click={() => {
              currency = currencyItem;
              dispatch2("currency", {
                currency: currencyItem,
              });
            }}>{currencyItem}</DropdownItem
          >
        {/each}
      </DropdownMenu>
    </Dropdown>
  </div>
  <br />
  <hr />
  <br />

  <div>
    <h2>Room</h2>
    <span>Add room packages and information about each room.</span>
  </div>

  {#each rooms as room, index}
    <RoomDetailsForm
      onClickRemove={() => {
        rooms.splice(index, 1);
        rooms = rooms;
      }}
      {room}
      {index}
      {currency}
      on:onRoomQtyChanged={(data) => {
        room[index] = room[index];
      }}
    />
  {/each}
  <div class="pt-3 pb-3 fw-bold">Total Sellable Rooms {getTotalRooms()}</div>
  <button class="btn pe-5 ps-5 mt-5" on:click={addNewRoom}
    >Add Room Category</button
  >
  <hr />
  <div class="mt-3 d-flex justify-content-between">
    <button
      on:click={() => {
        dispatch("rooms", {
          rooms: rooms,
        });
        onClickBack();
      }}
      class="btn pe-5 ps-5">Back</button
    >
    <button
      on:click={() => {
        dispatch("rooms", {
          rooms: rooms,
        });
        onClickNext();
      }}
      class="btn pe-5 ps-5">Next</button
    >
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
