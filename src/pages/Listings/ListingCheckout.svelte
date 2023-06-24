<!-- svelte-ignore a11y-click-events-have-key-events -->
<script>
  // @ts-nocheck
  import { onMount } from "svelte";
  import Finder from "../../shared/components/Finder/Finder.svelte";
  import Navbar from "../../shared/components/Navbar/Navbar.svelte";
  import { globalHistory } from "svelte-routing/src/history";
  import RoomCard from "../../shared/lib/RoomCard/RoomCard.svelte";
  import config from "../../../environment.json";
  import { Icon, Modal } from "sveltestrap";
  import { DateInput } from "date-picker-svelte";
  import Review from "../../shared/Forms/Review.svelte";
  export let location;
  export let id;
  id;
  let userid = sessionStorage.getItem("di");
  let pathname;
  let unsub;
  let selectedRoom = [];

  let numOfDevices = 1;
  let currentImageIndex = -1;

  $: listing = {};

  let loading = true;
  function fetchListingDetails(id) {
    if (id === "listing") {
      return;
    }
    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/listings/n/${pathname}`, {
      method: "GET",
    })
      .then((response) => response.text())
      .then((result) => {
        let arr = JSON.parse(result).filter((a) => a.id === parseInt(pathname));
        if (arr.length > 0) {
          listing = arr[0];
        }
        loading = false;
        loadStripeElements();
      })
      .catch((error) => error);
  }

  $: totalCartPrice = (
    selectedRoom.map((a) => a.room.Cost).length === 0
      ? [0, 0]
      : selectedRoom.map((a) => a.room.Cost * a.qty)
  ).reduce((a, b) => parseFloat(a) + parseFloat(b));
  onMount(() => {
    pathname = decodeURIComponent(location.pathname.split("/").reverse()[0]);
    fetchListingDetails(pathname);
    unsub = globalHistory.listen(({ location, action }) => {
      pathname = decodeURIComponent(location.pathname.split("/").reverse()[0]);
      fetchListingDetails(pathname);
    });
  });

  function checkout() {
    isCheckoutPopup = true;
  }

  let isCheckoutPopup = false;
  let information = {
    firstname: "",
    lastname: "",
    people: 1,
    adults: 1,
    children: 0,
    pets: false,
    contact_email: "",
    contact_tel: "",
    checkin_date: new Date(),
    checkout_date: new Date(),
    additional_requests: "",
  };
  let ERROR = false;
  const finaliseOrder = () => {
    loading = true;
    let html =
      `Name           : ${information.firstname} ${information.lastname}` +
      "<br/>";
    html += `Email          : ${information.contact_email}` + "<br/>";
    html += `Telephone      : ${information.contact_tel}` + "<br/>";
    html += `Persons        : ${information.people}` + "<br/>";
    html += `Check In date  : ${information.checkin_date}` + "<br/>";
    html += `Check Out date : ${information.checkout_date}` + "<br/>";
    html += "--------------------------------------<br/>";
    html += selectedRoom.map(
      (roomItem) =>
        `${roomItem.qty}x ${roomItem.room.RoomCategory} for ${roomItem.room.Cost} from ${listing.headline}<br/>`
    );
    html += "--------------------------------------<br/>";
    html += "[Total : " + totalCartPrice + " " + listing.currency + "]";
    html += "<br/>===================================<br/>";
    html += "Thank you for booking with DMT";

    // Add the booking to the table
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    var raw = JSON.stringify({
      Booking_Details: html,
      Customer_Email: information.contact_email,
      Customer_Tel: information.contact_tel,
      Persons: information.people,
      Invoice_amount: totalCartPrice,
      Approved: false,
      listings: {
        id: pathname,
        currency: listing.currency,
      },
    });

    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: raw,
      redirect: "follow",
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/bookings`, requestOptions)
      .then((response) => response.text())
      .then((result) => {
        if (JSON.parse(result).statusCode === 400) {
          loading = false;
          isCheckoutPopup = false;
          ERROR = true;
          return;
        }

        // Send email notification to the client
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        var raw = JSON.stringify({
          from: "Mandeep Dahal, <no-reply@gmail.com>",
          to: information.contact_email,
          html: html,
          subject: "DMT Adventures (Pvt.) Ltd.",
        });
        var requestOptions = {
          method: "POST",
          headers: myHeaders,
          body: raw,
          redirect: "follow",
        };
        fetch(
          `${config.SERVER_IP}${config.SERVER_PORT}/mailer/send`,
          requestOptions
        )
          .then((response) => response.text())
          .then((result) => {
            selectedRoom = [];
            loading = false;
            isCheckoutPopup = false;
          })
          .catch((error) => error);
      })
      .catch((error) => error);
  };

  let expandedImageUrl = "";
</script>

<div style="height: 100vh;width: 100%;overflow-x: hidden;">
  {#if expandedImageUrl === ""}
    {#if !loading}
      <div class="bg-white">
        <div class="container">
          <Navbar color={"text-dark"} btnTheme={""} />
        </div>

        <Finder levitating={false} isPopupOpen={isCheckoutPopup} />

        <div class="container mt-5">
          <div class="row">
            <div class="col-md-7" style="color: #434859;">
              <h3 class="fw-bolder" style="text-transform: capitalize;">
                {listing.headline}
              </h3>
              <span>
                <i class="bi bi-geo-alt-fill main-color" />
                <span style="text-transform: capitalize;"
                  >{listing.address}</span
                >
              </span>
            </div>
            <div class="col-md-5 text-sm-end text-center mt-4" />
          </div>
        </div>
        <br />
        <div class="">
          <div
            style="width: 100%;overflow-x:hidden"
            class="container-fluid ps-lg-1 pe-lg-1"
          >
            <div class="row">
              <div class="col-xl-6 mb-sm-4 mb-4" style="height: 460pt;">
                <div
                  on:click={() =>
                    // (expandedImageUrl = listing.listingImages[0]?.address)}
                    (currentImageIndex = 0)}
                  class="imageSpanned"
                  style="cursor: pointer;background-image: url({listing
                    .listingImages?.length > 0
                    ? config.SERVER_IP +
                      config.SERVER_PORT +
                      listing.listingImages[0]?.address
                    : ''});background-color:whitesmoke;"
                />
              </div>
              <div class="col-xl-6">
                <div class="row">
                  <div
                    class="col-sm-6"
                    style="height: 220pt;margin-bottom: 5pt;"
                  >
                    <div
                      on:click={() =>
                        // (expandedImageUrl = listing.listingImages[1]?.address)}
                        (currentImageIndex = 1)}
                      class="imageSpanned"
                      style="cursor: pointer;background-image: url({listing
                        .listingImages?.length > 1
                        ? config.SERVER_IP +
                          config.SERVER_PORT +
                          listing.listingImages[1]?.address
                        : ''});background-color:whitesmoke;"
                    />
                  </div>
                  <div
                    class="col-sm-6"
                    style="height: 220pt;margin-bottom: 20pt;"
                  >
                    <div
                      on:click={() =>
                        // (expandedImageUrl = listing.listingImages[2]?.address)}
                        (currentImageIndex = 2)}
                      class="imageSpanned"
                      style="cursor: pointer;background-image: url({listing
                        .listingImages?.length > 2
                        ? config.SERVER_IP +
                          config.SERVER_PORT +
                          listing.listingImages[2]?.address
                        : ''});background-color:whitesmoke;"
                    />
                  </div>
                </div>
                <div class="row">
                  <div
                    class="col-sm-6"
                    style="height: 220pt;margin-bottom: 10pt;"
                  >
                    <div
                      on:click={() => (currentImageIndex = 3)}
                      class="imageSpanned"
                      style="cursor: pointer;background-image: url({listing
                        .listingImages?.length > 3
                        ? config.SERVER_IP +
                          config.SERVER_PORT +
                          listing.listingImages[3]?.address
                        : ''});background-color:whitesmoke;"
                    />
                  </div>
                  <div
                    class="col-sm-6"
                    style="height: 220pt;margin-bottom: 10pt;"
                  >
                    <div
                      on:click={() => (currentImageIndex = 4)}
                      class="imageSpanned"
                      style="cursor: pointer;background-image: url({listing
                        .listingImages?.length > 4
                        ? config.SERVER_IP +
                          config.SERVER_PORT +
                          listing.listingImages[4]?.address
                        : ''});background-color:whitesmoke;"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <br />
        <div class="container">
          <div class="row">
            <div class="col-md-7 p-4">
              <hr />
              <h2>Hotel Description</h2>
              <div style="font-size: large;">{listing.description}</div>
              <hr />
              <br />
              {#if listing.isOnsite}
                <h3>
                  {listing?.rooms?.length === 0
                    ? "No Rooms available"
                    : "Choose your room"}
                </h3>

                <div class="row mt-5">
                  {#each listing?.rooms ?? [] as room}
                    <div class="col-lg-6 mb-3">
                      <RoomCard
                        {room}
                        currency={listing.currency}
                        onClickAddtoCart={() => {
                          selectedRoom = [
                            ...selectedRoom,
                            {
                              qty: 1,
                              room: {
                                ...room,
                                Cost: room.discountedEnable
                                  ? room.DiscountedPrice
                                  : room.Cost,
                              },
                            },
                          ];
                        }}
                      />
                    </div>
                  {/each}
                </div>
              {:else}
                <h2>Online Experience</h2>
                <div>{listing.headline}</div>
                <div>{listing.description}</div>
                <hr />
                <div>
                  <i class="bi bi-geo-alt-fill main-color" />
                  Happening at
                </div>
                <div>{listing.address}</div>
              {/if}

              <div>
                <br />
                <br />
                <Review user_id={userid} {listing} />
              </div>
            </div>
            <div class="col-md-5 p-5">
              <div class="sticky-top pt-5">
                <div class="card shadow p-3">
                  {#if listing.isOnsite}
                    <h3>Selected Rooms</h3>
                    <hr />
                    {#if selectedRoom.length === 0}
                      No Rooms selected
                    {/if}
                    {#each selectedRoom as room, index}
                      <div class="row">
                        <div class="col-9">
                          <div
                            class="d-flex align-items-start justify-content-start"
                          >
                            <button
                              class="btn-close me-2"
                              on:click={() => {
                                selectedRoom.splice(index, 1);
                                selectedRoom = selectedRoom;
                              }}
                            />
                            <div>
                              <div style="margin: 0pt;">
                                <b>{room.room.RoomCategory}</b>
                              </div>
                              <span style="font-size: small;margin: 0pt;">
                                {room.room.RoomDescription.length > 50
                                  ? room.room.RoomDescription.substring(0, 50) +
                                    "..."
                                  : room.room.RoomDescription}
                              </span>
                              <div
                                style="font-size: small;margin: 0pt;margin-top: 5pt;"
                              >
                                <div>{room.room.Cost} {listing.currency}</div>
                              </div>
                            </div>
                          </div>
                        </div>
                        <div class="col-3">
                          <input
                            type="number"
                            class="form-control"
                            bind:value={room.qty}
                            min="1"
                            max={room.room.Quantity}
                          />
                        </div>
                      </div>
                    {/each}
                    <hr />
                    <div class="d-flex justify-content-between mb-2">
                      <div><b>{"Total"}</b></div>
                      <div>{totalCartPrice} {listing.currency}</div>
                    </div>
                    {#if totalCartPrice !== 0}
                      <button class="btn btn-light border" on:click={checkout}>
                        Book Now
                      </button>
                    {/if}
                  {:else}
                    <h3>Book Experience</h3>
                    <span>
                      {listing.pricePerDevice}
                      {listing.currency} / device
                    </span>
                    <input
                      bind:value={numOfDevices}
                      type="number"
                      placeholder="No. of devices"
                      class="form-control mt-2"
                      min="1"
                    />
                    <div class="py-3 text-end">
                      Grand Total: {numOfDevices * listing.pricePerDevice}
                      {listing.currency}
                    </div>
                    <button class="btn mt-1"> Book now </button>
                  {/if}
                </div>
              </div>
            </div>
          </div>
        </div>
        <div style="height: 40vh;" />
      </div>
    {:else}
      <div class="p-5">
        <div
          class="d-flex align-items-center justify-content-center flex-column"
          style="height: 80vh;overflow: hidden;"
        >
          <div class="spinner-border text-secondary" />
          <div class="h4 mt-4">Loading, Please wait...</div>
        </div>
      </div>
    {/if}
  {/if}
</div>

<Modal isOpen={ERROR}>
  <div class="modal-header">
    <button class="btn btn-close" on:click={() => (ERROR = false)} />
  </div>
  <div class="modal-body">Cannot process with payment, try again.</div>
</Modal>

<Modal isOpen={isCheckoutPopup} centered={true}>
  <div class="modal-header">
    <div class="h4">Provide your check-in details</div>
    <button
      class="btn btn-close"
      on:click={() => (isCheckoutPopup = !isCheckoutPopup)}
    />
  </div>
  <div class="modal-body">
    {#if !loading}
      <div class="container">
        <div class="row mb-2">
          <div class="col-12">
            <input
              class="form-control"
              type="text"
              placeholder="First Name"
              bind:value={information.firstname}
            />
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-12">
            <input
              class="form-control"
              type="text"
              placeholder="Last Name"
              bind:value={information.lastname}
            />
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-12">
            <input
              class="form-control"
              type="text"
              placeholder="Email Address"
              bind:value={information.contact_email}
            />
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-md-6">
            <div class="mb-1">Check In</div>
            <DateInput
              min={new Date()}
              bind:value={information.checkin_date}
              on:select={() => {
                if (information.checkin_date >= information.checkout_date) {
                  information.checkout_date = information.checkin_date;
                }
              }}
              closeOnSelection={true}
              format={"dd-MM-yyyy"}
            />
          </div>
          <div class="col-md-6">
            <div class="mb-1">Check Out</div>
            <DateInput
              min={information.checkin_date}
              bind:value={information.checkout_date}
              closeOnSelection={true}
              format={"dd-MM-yyyy"}
            />
          </div>
        </div>

        <div class="row mb-2">
          <div class="col-md-8">
            <div class="mb-1">Telephone</div>
            <input
              class="form-control"
              type="text"
              placeholder="Telephone"
              bind:value={information.contact_tel}
            />
          </div>
          <div class="col-md-4">
            <div class="mb-1">No. Persons</div>
            <div class="form-control">
              {information.adults + information.children}
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-12">
            <div class="mb-1">Adults</div>
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-8">
            <input
              class="form-control"
              type="number"
              min="1"
              placeholder="Adults"
              bind:value={information.adults}
            />
          </div>
          <div class="col-4">
            <div class="d-flex align-items-center justify-content-around">
              <button
                class="border-0 bg-transparent"
                on:click={() => {
                  if (information.adults > 1) {
                    information.adults = information.adults - 1;
                    information.people = information.people - 1;
                  }
                }}
              >
                <i class="bi bi-dash-circle fs-3" />
              </button>
              <button
                class="border-0 bg-transparent"
                on:click={() => {
                  if (information.adults < 10) {
                    information.adults = information.adults + 1;
                    information.people = information.people + 1;
                  }
                }}
              >
                <i class="bi bi-plus-circle fs-3" />
              </button>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-12">
            <div class="mb-1">Childrens</div>
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-8">
            <input
              class="form-control"
              type="number"
              min="0"
              placeholder="Childrens"
              bind:value={information.children}
            />
          </div>
          <div class="col-4">
            <div class="d-flex align-items-center justify-content-around">
              <button
                class="border-0 bg-transparent"
                on:click={() => {
                  if (information.children > 0) {
                    information.children = information.children - 1;
                    information.people = information.people - 1;
                  }
                }}
              >
                <i class="bi bi-dash-circle fs-3" />
              </button>
              <button
                class="border-0 bg-transparent"
                on:click={() => {
                  if (information.children < 10) {
                    information.children = information.children + 1;
                    information.people = information.people + 1;
                  }
                }}
              >
                <i class="bi bi-plus-circle fs-3" />
              </button>
            </div>
          </div>
        </div>

        <div class="row mb-2">
          <div class="col-7">
            <div class="d-flex align-items-center h-100">
              <span class="text-center">Pets</span>
            </div>
          </div>
          <div class="col-5">
            <div class="d-flex align-items-center justify-content-around">
              <div class="form-check">
                <input
                  class="form-check-input"
                  type="radio"
                  name="flexRadioDefault"
                  id="flexRadioDefault1"
                />
                <label class="form-check-label" for="flexRadioDefault1">
                  Yes
                </label>
              </div>
              <div class="form-check ms-2">
                <input
                  class="form-check-input"
                  type="radio"
                  name="flexRadioDefault"
                  id="flexRadioDefault2"
                  checked
                />
                <label class="form-check-label" for="flexRadioDefault2">
                  No
                </label>
              </div>
            </div>
          </div>
        </div>

        <div class="row mb-2">
          <div class="col-12">
            <textarea
              class="form-control"
              type="text"
              placeholder="Additional Requests"
              bind:value={information.additional_requests}
            />
          </div>
        </div>
      </div>
    {:else}
      <div class="d-flex justify-content-center flex-column align-items-center">
        <div class="spinner-border" />
        <div style="font-size: 20pt;margin-top: 5pt;">Checking out...</div>
      </div>
    {/if}
  </div>
  <div class="modal-footer">
    <button
      class="btn btn-sm"
      on:click={() => (isCheckoutPopup = !isCheckoutPopup)}>Cancel</button
    >
    {#if information.people > 0 && information.firstname !== "" && information.lastname !== "" && information.contact_tel !== "" && information.contact_email !== "" && information.checkin_date !== "" && information.checkout_date !== ""}
      <button class="btn btn-sm" on:click={finaliseOrder}>Checkout</button>
    {/if}
  </div>
</Modal>

<Modal
  size="xl"
  centered
  isOpen={currentImageIndex !== -1}
  style="position:absolute;z-index: 3000;-webkit-user-select: none;user-select: none;"
>
  <div class="modal-header">
    <button class="btn btn-close" on:click={() => (currentImageIndex = -1)} />
  </div>
  <div class="modal-body overflow-hidden">
    ::{currentImageIndex}::
    <img
      src={config.SERVER_IP +
        config.SERVER_PORT +
        listing.listingImages[currentImageIndex]?.address}
      alt=""
      style="width: 100%;object-fit:contain;border-radius:10pt;"
    />
  </div>
  <div class="modal-foooter">
    <div class="d-flex justify-content-end p-3">
      <button
        disabled={currentImageIndex === 0}
        on:click={() => (currentImageIndex = currentImageIndex - 1)}
        class="px-3 btn btn-light border me-3">Previous</button
      >
      <button
        class="px-3 btn btn-light border"
        disabled={currentImageIndex === 4}
        on:click={() => (currentImageIndex = currentImageIndex + 1)}
        >Next</button
      >
    </div>
  </div>
</Modal>

<style>
  .imageSpanned {
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    height: 100%;
    width: 100%;
  }

  .main-color {
    color: #6801c8;
  }
  .main-color:first-child {
    color: #6801c8;
    border-color: #6801c8;
  }
  .main-color:nth-child(n):hover {
    color: #6801c8;
    border-color: #6801c8;
  }
</style>
