<script>
  import { createEventDispatcher } from "svelte";

  let event = createEventDispatcher();

  export let onClickBack;
  export let onClickNext;

  export let isIndividual = true;
  export let isOnsite = true;
  export let property = true;

  export let IndividualIdentificationNumber = "";
  export let IndividualTaxFileNumber = "";
  export let CompanyIdentificationNumber = "";
  export let CompanyTaxFileNumber = "";
</script>

<div class="mt-3">
  <div class="row">
    <div class="col-md-10">
      <h2>Business Nature</h2>
      <span>
        Please provide accurate business details. Failing to comply might result
        in suspension of your listing.
      </span>
      <div>
        <br />
        <br />
        <h4>Are you an Individual or an incorporated business?</h4>
        <div class="py-3">
          <div class="d-flex align-items-center">
            <!-- on:click={() => {
                  property = true;
                  event("isPropery", {
                    isProperty: property,
                  });
                }} -->
            <input
              bind:group={isIndividual}
              name="isIndividual"
              checked
              value={true}
              class="mt-3 mb-2 me-3 radio"
              id="welcom_property_checkbox"
              type="radio"
            />
            <label for="welcom_property_checkbox">Individual</label>
          </div>
          <div class="d-flex align-items-center">
            <!-- on:click={() => {
                  property = false;
                  event("isPropery", {
                    isProperty: property,
                  });
                }} -->
            <input
              bind:group={isIndividual}
              name="isIndividual"
              value={false}
              class="mt-3 mb-3 me-3"
              id="welcom_service_checkbox"
              type="radio"
            />
            <label for="welcom_service_checkbox">Incorporated</label>
          </div>
          {#if isIndividual}
            <input
              bind:value={IndividualIdentificationNumber}
              class="form-control mb-2"
              placeholder="Identification Number"
            />
            <input
              bind:value={IndividualTaxFileNumber}
              class="form-control mb-2"
              placeholder="Individual Tax file Number"
            />
          {:else if !isIndividual}
            <input
              bind:value={CompanyIdentificationNumber}
              class="form-control mb-2"
              placeholder="Company Registration Number"
            />
            <input
              bind:value={CompanyTaxFileNumber}
              class="form-control mb-2"
              placeholder="Company Tax file Number"
            />
          {/if}
        </div>
      </div>
      <div>
        <div>
          <br />
          <br />
          <h4>Is it a Property or a Service?</h4>
          <div class="mt-2" style="font-size: large;">
            <div class="mt-2" style="font-size: large;">
              <div class="d-flex align-items-center">
                <input
                  bind:group={property}
                  checked
                  name="property"
                  value={true}
                  class="mt-3 mb-3 me-3 radio"
                  id="welcom_property_checkbox1"
                  type="radio"
                />
                <label for="welcom_property_checkbox1">Accomodation</label>
              </div>
              <div class="d-flex align-items-center">
                <input
                  bind:group={property}
                  name="property"
                  value={false}
                  class="mt-3 mb-3 me-3"
                  id="welcom_service_checkbox1"
                  type="radio"
                />
                <label for="welcom_service_checkbox1">Experience</label>
              </div>
              {#if property !== true}
                <div class="ps-2">
                  <div class="d-flex align-items-center">
                    <input
                      bind:group={isOnsite}
                      name="isOnline"
                      value={false}
                      class="mt-3 mb-3 me-3"
                      id="experience_online_radio"
                      type="radio"
                    />
                    <label for="experience_online_radio">
                      <span class="bi-person-video" style="color: #6801c8;" />
                      Online</label
                    >
                  </div>
                  <div class="d-flex align-items-center">
                    <input
                      bind:group={isOnsite}
                      name="isOnline"
                      value={true}
                      class="mt-3 mb-3 me-3"
                      id="experience_onsite_radio"
                      type="radio"
                    />
                    <label for="experience_onsite_radio">
                      <span class="bi-people-fill" style="color: #6801c8;" />
                      Onsite</label
                    >
                  </div>
                </div>
              {/if}
            </div>
          </div>
        </div>
      </div>

      <div class="mt-3 d-flex justify-content-between">
        <button on:click={onClickBack} class="btn pe-5 ps-5">Back</button>
        <button
          on:click={() => {
            onClickNext({
              isIndividual: isIndividual,
              property: property,
              isOnsite: isOnsite,
              IndividualIdentificationNumber: IndividualIdentificationNumber,
              IndividualTaxFileNumber: IndividualTaxFileNumber,
              CompanyIdentificationNumber: CompanyIdentificationNumber,
              CompanyTaxFileNumber: CompanyTaxFileNumber,
            });
          }}
          class="btn pe-5 ps-5">Next</button
        >
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
