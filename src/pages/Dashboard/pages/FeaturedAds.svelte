<script>
  import { Modal } from "sveltestrap";
  import config from "../../../../environment.json";
  let listingData = [];
  let searchString = "";
  let userid = sessionStorage.getItem("di");
  var requestOptions = {
    method: "GET",
  };

  function filteredByString(string, list) {
    if (list?.length === 0 || list?.length === undefined) {
      return [];
    }
    return list?.filter((a) =>
      a.headline.toLowerCase().includes(string.toLowerCase())
    );
  }

  let isLoading = false;
  let loadData = () => {
    isLoading = true;
    fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/listings/${userid}`,
      requestOptions
    )
      .then((response) => response.json())
      .then((result) => {
        listingData = result;
        isLoading = false;
      })
      .catch((error) => {
        isLoading = false;
      });
  };

  loadData();
  let choosingPlan = false;
  const selectPlan = () => {
    choosingPlan = true;
  };
</script>

<div class="p-3">
  <div class="d-flex justify-content-between">
    <h2>Choose listing for featured</h2>
    <button class="btn btn-primary border px-4 fw-bold" on:click={selectPlan}
      >Featured Plans</button
    >
  </div>
  <div>
    These images will appear on the home page of DMT Tourism, so choose the best
    images for your listing
  </div>
  <span>
    Charges are applied for this feature, <button
      class="btn-link border-0 bg-transparent"
      on:click={selectPlan}
    >
      choose your plan
    </button>
  </span>
  <!-- Listings here -->

  <div class="container-fluid mt-4">
    <div class="d-flex justify-content-between">
      <h3>
        {filteredByString(searchString, listingData)?.length} Listing{filteredByString(
          searchString,
          listingData
        )?.length === 1
          ? ""
          : "s"}
      </h3>
    </div>
    <div class="col-md-3 mt-3">
      <input
        type="text"
        placeholder="Search"
        class="form-control"
        bind:value={searchString}
        style="background: none;"
      />
    </div>
    <br />
    <div class="container-fluid">
      {#if !isLoading}
        <table class="table table-light">
          <thead>
            <tr>
              <th>Listing</th>
              <th>Accomodation Type</th>
              <th>Description</th>
              <th>Room Categories</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {#each filteredByString(searchString, listingData) as listing}
              <tr>
                <td>
                  <i>{listing.headline}</i>
                </td>
                <td>
                  <div>
                    {listing.accomodationType}
                  </div>
                </td>
                <td>
                  {listing.description}
                </td>
                <td> {listing?.rooms?.length}</td>
                <td>
                  <button class="btn btn-light border">
                    Feature Listing
                  </button>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      {:else}
        <div
          class="p-5 d-flex align-items-center justify-content-center flex-column"
        >
          <div class="spinner-border text-dark mb-3" />
          <div class="">Loading Listings</div>
        </div>
      {/if}
    </div>
  </div>
</div>

<Modal centered size="xl" backdrop="static" isOpen={choosingPlan}>
  <div class="modal-header">Choose you featuring Plan</div>
  <div class="modal-body">Plans here.</div>
  <div class="modal-footer">
    <button
      class="btn btn-danger"
      on:click={() => {
        choosingPlan = false;
      }}>Cancel</button
    >
    <button
      class="btn btn-primary"
      on:click={() => {
        choosingPlan = false;
      }}>Subscribe</button
    >
  </div>
</Modal>
