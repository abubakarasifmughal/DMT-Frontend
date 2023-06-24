<script>
  import { Modal } from "sveltestrap";
  import config from "../../../../environment.json";
  let listingData = [];
  let searchString = "";

  let featureListingTill;
  let choosingFeature = false;
  let featuredListingID = "";

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
  <h3>Feature your listing</h3>

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
                  {#if listing?.featuredTill > Date.now()}
                    <button
                      class="btn btn-danger border"
                      on:click={async () => {
                        if (
                          confirm("Are you sure you want to cancel featured?")
                        ) {
                          let headersList = {
                            Accept: "*/*",
                            "User-Agent":
                              "Thunder Client (https://www.thunderclient.com)",
                            "Content-Type": "application/json",
                          };

                          let bodyContent = JSON.stringify({
                            featureTill: 0,
                          });

                          let response = await fetch(
                            `${config.SERVER_IP}/listings/feature/${listing.id}`,
                            {
                              method: "POST",
                              body: bodyContent,
                              headers: headersList,
                            }
                          );

                          let data = await response.text();
                          loadData();
                        }
                      }}
                    >
                      Cancel Feature
                    </button>
                  {:else}
                    <button
                      class="btn btn-light border"
                      on:click={() => {
                        choosingFeature = true;
                        featuredListingID = listing.id;
                      }}
                    >
                      Feature Listing
                    </button>
                  {/if}
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

<Modal centered backdrop="static" isOpen={choosingFeature}>
  <div class="modal-header">Choose featuring span</div>
  <div class="modal-body">
    <div><b>Feature your listing till</b></div>
    <input
      type="date"
      class="form-control my-2"
      bind:value={featureListingTill}
    />
  </div>
  <div class="modal-footer">
    <button class="btn btn-danger" on:click={() => (choosingFeature = false)}>
      Close
    </button>
    <button
      class="btn btn-primary"
      on:click={async () => {
        let headersList = {
          Accept: "*/*",
          "User-Agent": "Thunder Client (https://www.thunderclient.com)",
          "Content-Type": "application/json",
        };

        let bodyContent = JSON.stringify({
          featureTill: new Date(featureListingTill).getTime(),
        });

        let response = await fetch(
          `${config.SERVER_IP}/listings/feature/${featuredListingID}`,
          {
            method: "POST",
            body: bodyContent,
            headers: headersList,
          }
        );

        let data = await response.text();
        choosingFeature = false;
        featuredListingID = "";
        loadData();
      }}
    >
      Feature
    </button>
  </div>
</Modal>

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
