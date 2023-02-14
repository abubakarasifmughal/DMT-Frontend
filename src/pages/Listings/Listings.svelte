<script>
  import Finder from "../../shared/components/Finder/Finder.svelte";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import ListingCard from "../../shared/lib/ListingCard/ListingCard.svelte";
  import Mapbox from "../../shared/lib/MapBox/Mapbox.svelte";
  import config from "../../../environment.json";
  let user_id = sessionStorage.getItem("di");

  let loading = true;
  let listings = [
    // {
    //   isPropery: true,
    //   address: "Gold Ave, 55679",
    //   address_lat: null,
    //   address_lon: null,
    //   headline: "Hotel Grand",
    //   description:
    //     "Lorem ipsum dolor sit amet consectetur adipisicing elit. Corporis rem cum natus et facere deserunt voluptatem ipsum nostrum, aperiam itaque tenetur iusto suscipit ducimus in sed beatae nobis laboriosam accusamus!",
    //   accomodationType: "",
    //   currency: "",
    //   rooms: [],
    //   listingImages: [],
    // },
  ];

  var requestOptions = {
    method: "GET",
  };

  fetch(
    `${config.SERVER_IP}${config.SERVER_PORT}/listings/norast`,
    requestOptions
  )
    .then((response) => response.text())
    .then((result) => {
      listings = JSON.parse(result);
      loading = false;
    })
    .catch((error) => console.log("error", error));
</script>

<!-- const mapbox_token =
    'pk.eyJ1Ijoid2VicmV2aXZlZCIsImEiOiJjbDNuNDd6MG0wOXdtM3JzOWZjdDFzdWVuIn0.0RW0ALlkUzoBG1T2S1Eg9w'; -->

<!-- 

   const MAP_OPTIONS: any = {
      zoomSnap: 0.25,
      zoomDelta: 4,
      wheelPxPerZoomLevel: 60
    };

    const L = await import('leaflet');
    map = new L.Map('map', MAP_OPTIONS).setView([27.717245, 85.323959], 11);
 -->
{#if !loading}
  <div style="height: 100vh;width: 100%;overflow-x: hidden;">
    <div class="bg-white">
      <Navbar color={"text-dark"} btnTheme={""} />
      <Finder levitating={false} />
    </div>
    <div class="row" style="border-top: 1px solid gainsboro;">
      <div class="col-md-4 p-4" style="overflow: scroll;height: 80vh;">
        {#if listings.length === 0}
          <div>
            <h3>No Results</h3>
            <div>
              Try changing or removing some of your filters or adjusting your
              search area.
            </div>
            <hr />
          </div>
        {:else}
          {#each listings as listing}
            <ListingCard {listing} />
          {/each}
        {/if}
      </div>
      <div class="col-md-8 sticky" style="position: relative;">
        <Mapbox />
        <!-- <Map /> -->
      </div>
    </div>
  </div>
{:else}
  <div style="height: 100vh;width: 100%;overflow-x: hidden;">
    <div class="p-5">
      <div
        class="d-flex align-items-center justify-content-center flex-column"
        style="height: 80vh;overflow: hidden;"
      >
        <div class="spinner-border text-secondary" />
        <div class="h4 mt-4">Loading, Please wait...</div>
      </div>
    </div>
  </div>
{/if}
