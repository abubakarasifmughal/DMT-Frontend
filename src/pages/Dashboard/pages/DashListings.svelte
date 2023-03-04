<script>
  import Stepper from "../../../shared/components/Stepper/Stepper.svelte";
  import Amenities from "../../../shared/Forms/Amenities.svelte";
  import DetailsForListing from "../../../shared/Forms/DetailsForListing.svelte";
  import HotelDetails from "../../../shared/Forms/HotelDetails.svelte";
  import LocationForListing from "../../../shared/Forms/LocationForListing.svelte";
  import PhotosForProperty from "../../../shared/Forms/PhotosForProperty.svelte";
  import Welcome from "../../../shared/Forms/Welcome.svelte";
  import Progressbar from "../../../shared/lib/Progressbar/Progressbar.svelte";
  import config from "../../../../environment.json";
  import BusinessNature from "../../../shared/Forms/BusinessNature.svelte";

  let currentPage = 1;
  let user_id = sessionStorage.getItem("di");
  $: newListing = {
    isProperty: true,
    address: "",
    address_lat: "nan",
    address_lon: "nan",
    headline: "",
    description: "",
    accomodationType: "",
    currency: "",
    rooms: [],
    listingImages: [],
    weeklyDiscount: 10,
    nightlyDiscount: 0,
    // New props, not in db or nest yet
    isIndividual: true,
    IndividualIdentificationNumber: "",
    IndividualTaxFileNumber: "",
    CompanyIdentificationNumber: "",
    CompanyTaxFileNumber: "",
  };

  function apiCall() {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    var raw = JSON.stringify({ ...newListing });

    const requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/listings/${user_id}`, {
      ...requestOptions,
    })
      .then((response) => response.text())
      .then((result) => {
        // -----
        newListing = {
          isProperty: true,
          address: "",
          address_lat: "nan",
          address_lon: "nan",
          headline: "",
          description: "",
          accomodationType: "",
          currency: "",
          rooms: [],
          listingImages: [],
          weeklyDiscount: 10,
          nightlyDiscount: 0,
          isIndividual: false,
          IndividualIdentificationNumber: "",
          IndividualTaxFileNumber: "",
          CompanyIdentificationNumber: "",
          CompanyTaxFileNumber: "",
        };
        // -----
        currentPage = 1;
      })
      .catch((error) => console.log("error", error));
  }
</script>

<div>
  <Progressbar total={6} current={currentPage} />
  <br />
  <hr />
  <div>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-2 ">
          <Stepper
            title={"Listings"}
            steps={[
              { label: "Welcome", substeps: [] },
              { label: "Location", substeps: [] },
              { label: "Business", substeps: [] },
              { label: "Details", substeps: [] },
              { label: "Photos", substeps: [] },
              { label: "Amenities", substeps: [] },
              { label: "Property", substeps: [] },
              { label: "Go Live", substeps: [] },
            ]}
            activeIndex={currentPage - 1}
            substepIndex={0}
          />
        </div>
        <div class="col-lg-10">
          {#if currentPage === 1}
            <Welcome onClickContinue={() => (currentPage = currentPage + 1)} />
          {:else if currentPage === 2}
            <LocationForListing
              coordX={27.717245}
              coordY={85.323959}
              locationAddress={newListing.address}
              on:AddressChanged={(data) => {
                newListing.address = data.detail.Address;
              }}
              onClickBack={() => (currentPage = currentPage - 1)}
              onClickNext={() => (currentPage = currentPage + 1)}
            />
          {:else if currentPage === 3}
            <BusinessNature
              IndividualTaxFileNumber={newListing.IndividualTaxFileNumber}
              IndividualIdentificationNumber={newListing.IndividualIdentificationNumber}
              CompanyIdentificationNumber={newListing.CompanyIdentificationNumber}
              CompanyTaxFileNumber={newListing.CompanyTaxFileNumber}
              isIndividual={newListing.isIndividual}
              property={newListing.isProperty}
              on:isPropery={(data) => {
                newListing.isProperty = data.detail.isProperty;
              }}
              onClickBack={() => (currentPage = currentPage - 1)}
              onClickNext={() => (currentPage = currentPage + 1)}
            />
          {:else if currentPage === 4}
            <DetailsForListing
              headline={newListing.headline}
              description={newListing.description}
              on:headline={(data) => {
                newListing.headline = data.detail.headline;
              }}
              on:description={(data) => {
                newListing.description = data.detail.description;
              }}
              onClickBack={() => (currentPage = currentPage - 1)}
              onClickNext={() => (currentPage = currentPage + 1)}
            />
          {:else if currentPage === 5}
            <HotelDetails
              currency={newListing.currency}
              on:currency={(data) => {
                newListing.currency = data.detail.currency;
              }}
              AccomodationType={newListing.accomodationType}
              on:AccomodationType={(data) => {
                newListing.accomodationType = data.detail.AccomodationType;
              }}
              rooms={newListing.rooms}
              on:rooms={(data) => {
                newListing.rooms = data.detail.rooms;
              }}
              onClickBack={() => (currentPage = currentPage - 1)}
              onClickNext={() => (currentPage = currentPage + 1)}
            />
          {:else if currentPage === 6}
            <PhotosForProperty
              on:listingImages={(data) => {
                newListing.listingImages = data.detail.images;
              }}
              images={newListing.listingImages}
              onClickBack={() => (currentPage = currentPage - 1)}
              onClickNext={apiCall}
            />
          {/if}
        </div>
      </div>
    </div>
  </div>
</div>
