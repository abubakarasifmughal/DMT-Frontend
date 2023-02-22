<script lang="ts">
  import {
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    FormGroup,
    Input,
  } from "sveltestrap";
  import { navigate } from "svelte-routing";

  import { DateInput } from "date-picker-svelte";

  export let levitating: boolean = true;
  let checkindate = new Date();
  // let checkindate = "2021-11-11 14:35";
  let checkoutdate = new Date();

  let searchString: string = "";
  let isOpen: boolean = false;
  let isOpen2: boolean = false;

  $: locations = [
    {
      label: "Anywhere in Nepal",
    },
    {
      label: "Kathmandu",
    },
    {
      label: "Pokhara",
    },
    {
      label: "Baharatpur",
    },
  ];

  let allServices = ["Accomodation", "Experience"];
  let serviceSelected = "";
  function getFilteredLocations(array: any[], searchString: string) {
    return array.filter((a: any) =>
      a.label.toLowerCase().includes(searchString.toLowerCase())
    );
  }

  export let isPopupOpen = false;
</script>

<div class="container">
  <div class="container">
    <div
      class="container {isPopupOpen ? '' : 'finderBox'} bg-white {levitating
        ? 'shadow'
        : ''} p-3"
      style="transform: {levitating ? 'translateY(-50%)' : ''};"
    >
      <div class="row">
        <div class="col-md-3">
          <Dropdown {isOpen} toggle={() => (isOpen = !isOpen)}>
            <DropdownToggle tag="div" class="col-12">
              <Input
                placeholder="Where"
                bind:value={searchString}
                style="border-radius: 3px;"
              />
            </DropdownToggle>
            <DropdownMenu style="width: 100%;">
              {#each getFilteredLocations(locations, searchString) as location}
                <DropdownItem on:click={() => (searchString = location.label)}
                  >{location.label}</DropdownItem
                >
              {/each}
            </DropdownMenu>
          </Dropdown>
        </div>
        <div class="col-md-3 ">
          <Dropdown
            isOpen={isOpen2}
            toggle={() => (isOpen2 = !isOpen2)}
            class={"mt-2 mt-md-0 mb-md-0 mb-1"}
          >
            <DropdownToggle tag="div" class="col-12">
              <Input
                placeholder="Service Type"
                bind:value={serviceSelected}
                style="border-radius: 3px;"
              />
            </DropdownToggle>
            <DropdownMenu style="width: 100%;">
              {#each allServices as service}
                <DropdownItem on:click={() => (serviceSelected = service)}
                  >{service}</DropdownItem
                >
              {/each}
            </DropdownMenu>
          </Dropdown>
        </div>
        <div class="col-md-4 ">
          <div class="row mt-2 mt-md-0 mb-md-0 mb-2">
            <div class="col-6" style="font-size: large;">
              <DateInput
                min={new Date()}
                bind:value={checkindate}
                on:select={() => {
                  if (checkindate >= checkoutdate) {
                    checkoutdate = checkindate;
                  }
                }}
                closeOnSelection={true}
                format={"dd-MM-yyyy"}
              />
            </div>
            <div class="col-6" style="font-size: large;">
              <DateInput
                min={checkindate}
                bind:value={checkoutdate}
                closeOnSelection={true}
                format={"dd-MM-yyyy"}
              />
            </div>
          </div>
        </div>
        <div class="col-md-2 d-flex align-item-center">
          <button
            on:click={() => {
              navigate("/listing");
            }}
            class="btn col-12"
            style="border-radius: 3px;"
          >
            Search
          </button>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .finderBox {
    position: relative;
    z-index: 2000;
    border-radius: 5pt;
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

  :root {
    --date-input-width: 100%;
    --date-picker-foreground: black;
    /* --date-picker-background */
    --date-picker-highlight-border: #9427f7;
    /* --date-picker-highlight-shadow: #9427f7; */
    --date-picker-selected-color: white;
    --date-picker-selected-background: #9427f7;
  }
</style>
