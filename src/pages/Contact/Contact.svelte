<script>
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import Checkbox from "../../shared/components/Checkbox/Checkbox.svelte";
  import { DateInput } from "date-picker-svelte";

  let checkboxes = [
    "Travel Planning",
    "Rental vehicle",
    "Airport transportation",
    "Trekking Info",
    "Tour Guides",
    "Hotel support",
  ];

  let altitudes = [
    { name: "1,000 ft", value: "1,000" },
    { name: "2,500 ft", value: "2,500" },
    { name: "5,000 ft", value: "5,000" },
    { name: "6,000 ft", value: "6,000" },
    { name: "10,000+ ft", value: "10,000+" },
  ];

  let references = [
    { name: "Facebook", value: "facebook" },
    { name: "Google", value: "google" },
    { name: "Instagram", value: "Instagram" },
    { name: "Refferal", value: "Refferal" },
  ];

  let startDate = new Date();
  let endDate = new Date();

  let selected = [];

  const handleCheckboxSelect = (name) => {
    const includesIndex = selected.indexOf(name);

    if (includesIndex > -1)
      return (selected = selected.filter((item) => item !== name));
    selected = [...selected, name];
  };
</script>

<div class="container">
  <Navbar color={"text-dark"} btnTheme={""} />
  <div class="d-flex justify-content-center my-5">
    <div class="col-8 col-md-8 col-sm-10 text-center">
      <div class="h2">Have a personal tour guide plan your trip.</div>
      <span>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Eaque est dicta
        aliquam a possimus cupiditate eum amet! Molestiae, quis quia.
      </span>
    </div>
  </div>
  <div class="container-fluid mb-4 px-5">
    <div class="h4 mb-4">Services Needed</div>
    <div class="row">
      {#each checkboxes as name, i}
        <button
          on:click={() => handleCheckboxSelect(name)}
          class="col-md-4 border-0 bg-transparent h-100 mb-4"
        >
          <Checkbox {name} checked={selected.includes(name)} />
        </button>
      {/each}
    </div>
  </div>
  <div class="container-fluid my-4 px-5 bot-spacer">
    <div class="h4 mb-4">More About You</div>
    <div class="row my-3">
      <div class="col-6">
        <input
          type="text"
          class="form-control py-3"
          id="firstName"
          placeholder="First Name"
        />
      </div>
      <div class="col-6">
        <input
          type="text"
          class="form-control py-3"
          id="lastName"
          placeholder="Last Name"
        />
      </div>
    </div>
    <div class="row my-3">
      <div class="col-6">
        <input
          type="email"
          class="form-control py-3"
          id="email"
          placeholder="Email"
        />
      </div>
      <div class="col-6">
        <input
          type="text"
          class="form-control py-3"
          id="phone"
          placeholder="Phone"
        />
      </div>
    </div>
    <div class="row my-3">
      <div class="col-6">
        <DateInput
          bind:value={startDate}
          closeOnSelection={true}
          format={"dd-MM-yyyy"}
        />
      </div>
      <div class="col-6">
        <DateInput
          bind:value={endDate}
          closeOnSelection={true}
          format={"dd-MM-yyyy"}
        />
      </div>
    </div>
    <div class="row my-3">
      <div class="col-6">
        <select class="form-select py-3">
          <option selected>Current Altitude</option>
          {#each altitudes as altitude}
            <option value={altitude.value}>{altitude.name}</option>
          {/each}
        </select>
      </div>
      <div class="col-6">
        <select class="form-select py-3">
          <option selected>How Did You Hear About Us</option>
          {#each references as reference}
            <option value={reference.value}>{reference.name}</option>
          {/each}
        </select>
      </div>
    </div>
    <div class="row my-3">
      <div class="col-12">
        <textarea
          class="form-control py-3"
          placeholder="Tell us about your trip..."
        />
      </div>
    </div>
    <div class="row my-3">
      <div class="col-12">
        <button type="button" class="btn btn-primary border-0 px-4 py-2"
          >Submit</button
        >
      </div>
    </div>
  </div>
</div>

<style>
  :root {
    --date-input-width: 100%;
    --date-picker-foreground: #9427f7;
    /* --date-picker-background */
    --date-picker-highlight-border: #9427f7;
    /* --date-picker-highlight-shadow: #9427f7; */
    --date-picker-selected-color: white;
    --date-picker-selected-background: #9427f7;
  }

  .btn-primary {
    background-color: #9c59df;
  }

  .bot-spacer {
    margin-bottom: 150px !important;
  }
</style>
