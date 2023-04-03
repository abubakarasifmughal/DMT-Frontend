<script lang="ts">
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
  import { Alert, Modal, ModalBody, ModalHeader } from "sveltestrap";
  export let hideCreationUI;

  let errors: string[] = [];
  let done: boolean = false;
  let currentPage = 1;
  let letsLoad = false;
  let user_id = sessionStorage.getItem("di");
  $: newListing = {
    isProperty: true,
    isOnsite: true,
    address: "",
    address_lat: "nan",
    address_lon: "nan",
    headline: "",
    description: "",
    pricePerDevice: 0,
    accomodationType: "",
    currency: "",
    rooms: [],
    listingImages: [],
    amenities: [],
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
    letsLoad = true;
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
          isOnsite: true,
          address: "",
          address_lat: "nan",
          address_lon: "nan",
          amenities: [],
          pricePerDevice: 0,
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
        done = true;

        // revert back to the listing page
      })
      .catch((error) => error);
  }
</script>

<Modal centered isOpen={done}>
  <div class="modal-header">Congratulations!</div>
  <div class="modal-body">Your listing was successfully created!</div>
  <div class="modal-footer">
    <button
      class="btn btn-light border"
      on:click={() => {
        done = false;
        hideCreationUI();
      }}>Close</button
    >
  </div>
</Modal>

{#if !letsLoad}
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
              <Welcome
                onClickContinue={() => (currentPage = currentPage + 1)}
              />
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
                isOnsite={newListing.isOnsite}
                IndividualIdentificationNumber={newListing.IndividualIdentificationNumber}
                CompanyIdentificationNumber={newListing.CompanyIdentificationNumber}
                CompanyTaxFileNumber={newListing.CompanyTaxFileNumber}
                isIndividual={newListing.isIndividual}
                property={newListing.isProperty}
                on:isPropery={(data) => {
                  newListing.isProperty = data.detail.isProperty;
                  newListing.isOnsite = data.detail.isOnsite;
                }}
                onClickBack={() => (currentPage = currentPage - 1)}
                onClickNext={({
                  isIndividual,
                  property,
                  IndividualIdentificationNumber,
                  IndividualTaxFileNumber,
                  CompanyIdentificationNumber,
                  CompanyTaxFileNumber,
                  isOnsite,
                }) => {
                  newListing.isIndividual = isIndividual;
                  newListing.isProperty = property;
                  newListing.isOnsite = isOnsite;
                  if (newListing.isProperty) {
                    newListing.isOnsite = true;
                  }
                  newListing.IndividualIdentificationNumber =
                    IndividualIdentificationNumber;
                  newListing.IndividualTaxFileNumber = IndividualTaxFileNumber;
                  newListing.CompanyIdentificationNumber =
                    CompanyIdentificationNumber;
                  newListing.CompanyTaxFileNumber = CompanyTaxFileNumber;

                  currentPage = currentPage + 1;
                }}
              />
            {:else if currentPage === 4}
              <DetailsForListing
                isOnsite={newListing.isOnsite}
                isListing={newListing.isProperty}
                selectedAmeneties={newListing.amenities}
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
                pricePerDevice={newListing.pricePerDevice}
                isOnsite={newListing.isOnsite}
                isProperty={newListing.isProperty}
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
                on:pricePerDevice={(data) => {
                  newListing.pricePerDevice = data.detail.pricePerDevice;
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
                onClickNext={() => {
                  let a = {
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
                    isIndividual: true,
                    IndividualIdentificationNumber: "",
                    IndividualTaxFileNumber: "",
                    CompanyIdentificationNumber: "",
                    CompanyTaxFileNumber: "",
                  };
                  if (newListing.headline === "") {
                    errors = [...errors, "Please provide a headline"];
                  }
                  if (newListing.isProperty) {
                    if (newListing.address === "") {
                      errors = [...errors, "Address cannot be empty"];
                    }
                  }
                  if (newListing.description === "") {
                    errors = [...errors, "You did not enter any description"];
                  }
                  if (newListing.accomodationType === "") {
                    errors = [
                      ...errors,
                      "Accomodation Type must be selected from Experience or Accomodation",
                    ];
                  }
                  if (newListing.currency === "") {
                    errors = [...errors, "Select your currency"];
                  }
                  if (newListing.isProperty) {
                    if (newListing.rooms.length <= 0) {
                      errors = [...errors, "No rooms were added"];
                    }
                  }
                  if (newListing.listingImages.length <= 0) {
                    errors = [
                      ...errors,
                      "The images for you listing, please add a few",
                    ];
                  }
                  if (newListing.isOnsite) {
                    if (newListing.amenities.length <= 0) {
                      errors = [
                        ...errors,
                        "There are no facilities for you listing, please add a few",
                      ];
                    }
                  }
                  if (errors.length <= 0) {
                    apiCall();
                  }
                }}
              />
            {/if}
          </div>
        </div>
      </div>
    </div>

    <Modal isOpen={errors.length > 0} backdrop="static">
      <ModalHeader class="row">
        <div class="row col-12">
          <div class="col-11 ">Please fix the following errors</div>
          <div class="col-1 ">
            <button
              class="btn btn-close"
              on:click={() => {
                errors = [];
                currentPage = 2;
              }}
            />
          </div>
        </div>
      </ModalHeader>
      <ModalBody>
        {#each errors as error}
          <Alert color="danger" dismissible>{error}</Alert>
        {/each}
      </ModalBody>
    </Modal>
  </div>
{:else}
  <div
    class="d-flex flex-column justify-content-center align-items-center"
    style="min-height: 50vh;width: 100%;"
  >
    <div class="spinner-border" />
    <br />
    <span>Creating Listing</span>
  </div>
{/if}

<style>
</style>
