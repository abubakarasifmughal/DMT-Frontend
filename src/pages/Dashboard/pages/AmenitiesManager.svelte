<script>
  import { Modal } from "sveltestrap";

  export let amenities = [];
  let newAmentityTitle = "";
  let newAmentityDesc = "";
  let newAmentityAspectTitle = "";

  let addToListingWithIndex = -1;

  let isOpen = false;
</script>

<div class="p-3">
  <br />
  <div class="row">
    <div class="col-lg-6">
      <div class="d-flex justify-content-between">
        <h2>Manage Amenities here</h2>
        <button class="btn btn-light border" on:click={() => (isOpen = !isOpen)}
          >Add New</button
        >
      </div>
      <br />
      <div class="card" style="max-height: 70vh;overflow-y: scroll;">
        <div class="card-title ">
          <div class="card-header text-center bg-white">
            <h3 class="pt-2">Amenities</h3>
          </div>
          <div class="card-body">
            {#if amenities.length === 0}
              <div class="text-center">No Amenities, try adding a few</div>
            {/if}
            {#each amenities as amenity, a_index}
              <div>
                <div class="d-flex">
                  <button
                    class="btn btn-close me-2"
                    on:click={() => {
                      amenities = amenities.filter(
                        (a) => a.label !== amenity.label
                      );
                    }}
                  />
                  <div class="d-flex flex-column">
                    <span class="fw-bold">{amenity.label}</span>
                    <span>{amenity.description}</span>
                  </div>
                </div>
                <ul class="py-2">
                  {#each amenity?.options as option}
                    <li>
                      <div class="btn-group mt-2 w-100">
                        <button
                          style="width: min-content;"
                          class="btn-sm btn btn-danger"
                          on:click={() => {
                            amenity.options = amenity?.options.filter(
                              (a) => a !== option
                            );
                          }}
                        >
                          Remove
                        </button>
                        <button
                          class="btn btn-sm btn-light border w-100"
                          disabled
                        >
                          {option.label}
                        </button>
                      </div>
                    </li>
                  {/each}
                  <li class="mt-2" style="list-style: none;">
                    <button
                      class="btn btn-sm btn-success px-4"
                      on:click={() => {
                        addToListingWithIndex = a_index;
                      }}>New</button
                    >
                  </li>
                </ul>
              </div>
            {/each}
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<Modal isOpen={addToListingWithIndex !== -1} backdrop="static" centered>
  <div class="modal-header">Add New Aspect</div>
  <div class="modal-body">
    <input
      type="text"
      class="form-control"
      bind:value={newAmentityAspectTitle}
    />
    <button
      class="mt-3 btn-danger btn"
      on:click={() => {
        addToListingWithIndex = -1;
      }}>Cancel</button
    >
    <button
      class="mt-3 btn-primary btn"
      on:click={() => {
        amenities[addToListingWithIndex].options = [
          {
            label: newAmentityAspectTitle,
            checked: false,
            id: 1,
          },
          ...amenities[addToListingWithIndex].options,
        ];
        addToListingWithIndex = -1;
      }}>Done</button
    >
  </div>
</Modal>

<Modal {isOpen} centered backdrop="static">
  <div class="modal-header">Add New Amenity to system</div>
  <div class="modal-body">
    <div class="container">
      <input
        type="text"
        bind:value={newAmentityTitle}
        class="form-control"
        placeholder="New Amenity Title"
      />
      <textarea
        bind:value={newAmentityDesc}
        class="form-control mt-2"
        placeholder="New Amenity Description"
      />
    </div>
  </div>
  <div class="modal-footer">
    <button class="btn btn-danger" on:click={() => (isOpen = !isOpen)}>
      Close
    </button>
    {#if newAmentityTitle}
      <button
        class="btn btn-primary"
        on:click={() => {
          amenities = [
            {
              label: newAmentityTitle,
              description: newAmentityDesc,
              options: [],
            },
            ...amenities,
          ];
          isOpen = !isOpen
        }}
      >
        Add Amenity
      </button>
    {/if}
  </div>
</Modal>
