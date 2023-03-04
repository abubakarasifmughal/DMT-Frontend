<script>
  import { createEventDispatcher } from "svelte";

  let event = createEventDispatcher();

  export let property = true;

  export let onClickContinue;
</script>

<div class="mt-3">
  <div class="row">
    <div class="col-md-6">
      <h2>Is it a Property or a Service?</h2>
      <span>
        Lorem ipsum dolor, sit amet consectetur adipisicing elit. Laboriosam
        autem fuga, ratione earum magnam maiores natus dolorem porro magni culpa
        placeat alias. Totam asperiores ducimus natus dolores, corporis ipsam
        officiis.
      </span>
      <div class="mt-2" style="font-size: large;">
        <div class="d-flex align-items-center">
            <!-- on:click={() => {
              property = true;
              event("isPropery", {
                isProperty: property,
              });
            }} -->
          <input
            group={property}
            name="property"
            value={true}
            class="mt-3 mb-3 me-3 radio"
            id="welcom_property_checkbox"
            type="radio"
          />
          <label for="welcom_property_checkbox">Property</label>
        </div>
        <div class="d-flex align-items-center">
            <!-- on:click={() => {
              property = false;
              event("isPropery", {
                isProperty: property,
              });
            }} -->
          <input
            group={property}
            name="property"
            value={false}
            class="mt-3 mb-3 me-3"
            id="welcom_service_checkbox"
            type="radio"
          />
          <label for="welcom_service_checkbox">Service</label>
        </div>
        <button on:click={onClickContinue} class="mt-3 btn pe-5 ps-5">
          Continue
        </button>
      </div>
    </div>
  </div>
</div>

<style>
  input[type="radio"] {
    transform: scale(1.3);
    accent-color: #9427f7;
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
