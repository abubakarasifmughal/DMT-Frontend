<script>
  import {
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    Modal,
    Offcanvas,
  } from "sveltestrap";
  import config from "../../../../environment.json";
  import Amenities from "../../../shared/Forms/Amenities.svelte";
  export let onClickCreate;
  let listingData = [];
  let searchString = "";
  let userid = sessionStorage.getItem("di");
  let AllAmeneties = [
    {
      label: "Property",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Smoking Area",
          checked: false,
          id: 1,
        },
        {
          label: "Shops",
          checked: false,
          id: 2,
        },
        {
          label: "Wedding Facilities",
          checked: false,
          id: 3,
        },
        {
          label: "Library",
          checked: false,
          id: 4,
        },
        {
          label: "Gym",
          checked: false,
          id: 5,
        },
        {
          label: "Spa",
          checked: false,
          id: 6,
        },
        {
          label: "Sauna",
          checked: false,
          id: 7,
        },
        {
          label: "Massage",
          checked: false,
          id: 8,
        },
        {
          label: "Poolside bar",
          checked: false,
          id: 9,
        },
      ],
    },
    {
      label: "Facilities",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Executive floot",
          checked: false,
          id: 10,
        },
        {
          label: "Business center",
          checked: false,
          id: 11,
        },
        {
          label: "Meetings Facilities",
          checked: false,
          id: 12,
        },
        {
          label: "Executive Lounge",
          checked: false,
          id: 13,
        },
      ],
    },
    {
      label: "Food and Drinks",
      description: `Add room packages and information about each room.`,
      options: [
        {
          label: "Cafe",
          checked: false,
          id: 14,
        },
        {
          label: "Restaurant",
          checked: false,
          id: 15,
        },
        {
          label: "Bar",
          checked: false,
          id: 16,
        },
        {
          label: "Room Service",
          checked: false,
          id: 17,
        },
      ],
    },
  ];
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

  let openForEdit = false;
  let listingToBeEdited;
  let toggleForEdit = (listing) => {
    listingToBeEdited = listing;
    openForEdit = !openForEdit;
  };
  let updateListing = () => {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");

    var requestOptions = {
      method: "PUT",
      headers: myHeaders,
      body: JSON.stringify(listingToBeEdited),
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/listings`, requestOptions)
      .then((response) => response.text())
      .then((result) => {
        listingToBeEdited = {};
        openForEdit = false;
        loadData();
      })
      .catch((error) => error);
  };

  let openForAddNewRoom = false;
  let toggleForAddNewRoom = (listing) => {
    listingToBeEdited = listing;
    listingToBeEdited.rooms = [];
    openForAddNewRoom = !openForAddNewRoom;
  };

  let isOpenAmenitiesSheet = false;

  const toggleAmenetiesSheet = () => {
    isOpenAmenitiesSheet = !isOpenAmenitiesSheet;
  };

  let addRoomToTheListing = () => {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var requestOptions = {
      method: "POST",
      headers: myHeaders,
      body: JSON.stringify(listingToBeEdited.rooms),
    };

    fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/room/add/${listingToBeEdited.id}`,
      requestOptions
    )
      .then((response) => response.text())
      .then((result) => {
        openForAddNewRoom = !openForAddNewRoom;
        loadData();
      })
      .catch((err) => {});
  };

  const deleteListing = (listing) => {
    let headersList = {
      Accept: "*/*",
      "User-Agent": "Thunder Client (https://www.thunderclient.com)",
    };

    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/listings/${listing.id}`, {
      method: "DELETE",
      headers: headersList,
    })
      .then((response) => {
        loadData();
      })
      .catch((err) => {
        alert("Could not delete");
        loadData();
      });
  };

  let openForRoomsEdit = false;
  let toggleForRoomsEdit = (listing) => {
    listingToBeEdited = listing;
    openForRoomsEdit = !openForRoomsEdit;
  };

  let onUpdateRoomsDetail = () => {
    var myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    var requestOptions = {
      method: "PUT",
      headers: myHeaders,
      body: JSON.stringify(listingToBeEdited),
    };
    fetch(`${config.SERVER_IP}${config.SERVER_PORT}/listings`, requestOptions)
      .then((response) => response.text())
      .then((result) => {
        listingToBeEdited = {};
        openForRoomsEdit = false;
        loadData();
      })
      .catch((error) => error);
  };

  let isLoading = false;
  let loadData = () => {
    isLoading = true;
    fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/listings/${userid}`,
      requestOptions
    )
      .then((response) => response.text())
      .then((result) => {
        listingData = JSON.parse(result);
        isLoading = false;
      })
      .catch((error) => {
        isLoading = false;
      });
  };

  let openForBookings = false;
  let bookingsData = {};
  const toggleForBookings = (listing) => {
    var requestOptions = {
      method: "GET",
    };

    fetch(
      `${config.SERVER_IP}${config.SERVER_PORT}/bookings/${listing.id}`,
      requestOptions
    )
      .then((response) => response.text())
      .then((result) => {
        bookingsData = {
          listing: listing,
          bookings: JSON.parse(result),
        };
        openForBookings = !openForBookings;
      })
      .catch((error) => error);
  };

  loadData();
</script>

<div class="container-fluid mt-4">
  <div class="d-flex justify-content-between">
    <h3>{filteredByString(searchString, listingData)?.length} Listings</h3>
    <button class="btn me-4" on:click={onClickCreate}>Create Listing</button>
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
                <Dropdown>
                  <DropdownToggle caret class="btn btn-light border"
                    >Actions</DropdownToggle
                  >
                  <DropdownMenu>
                    <DropdownItem on:click={() => toggleForEdit(listing)}>
                      Edit
                    </DropdownItem>
                    <DropdownItem on:click={() => toggleForAddNewRoom(listing)}>
                      Add new
                    </DropdownItem>
                    <DropdownItem on:click={() => toggleForRoomsEdit(listing)}>
                      Edit Rooms
                    </DropdownItem>
                    <DropdownItem on:click={() => toggleForBookings(listing)}>
                      Bookings
                    </DropdownItem>
                    <DropdownItem
                      class="text-danger"
                      on:click={() => deleteListing(listing)}
                      >Delete</DropdownItem
                    >
                  </DropdownMenu>
                </Dropdown>
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

<Modal isOpen={openForEdit} toggle={toggleForEdit} backdrop="static">
  <div class="modal-header">
    Update Listing
    <button class="btn btn-close bg-white border-0" on:click={toggleForEdit} />
  </div>
  <div class="modal-body">
    <div class="row">
      <div class="col-md-12">
        <input
          type="text"
          class="form-control mb-2"
          placeholder="headline"
          bind:value={listingToBeEdited.headline}
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <input
          type="text"
          class="form-control mb-2"
          placeholder="Description"
          bind:value={listingToBeEdited.description}
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <input
          type="text"
          class="form-control mb-2"
          placeholder="Accomodation Type"
          bind:value={listingToBeEdited.accomodationType}
        />
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <input
          type="text"
          class="form-control mb-2"
          placeholder="Currency"
          bind:value={listingToBeEdited.currency}
        />
      </div>
      <div class="row">
        <div class="col-md-12">
          <textarea
            class="form-control mb-2 col-12"
            placeholder="Description"
            bind:value={listingToBeEdited.address}
          />
        </div>
      </div>
    </div>
    <div class="row mb-3">
      <div class="col-md-12">
        {#each listingToBeEdited?.listingImages as image, index}
          <label
            class="udateImageContainer border"
            for="{index}_EditListingPopup"
          >
            {#if image.address.substring(0, 4) === "data"}
              <img
                src={`${image.address}`}
                class="shadow-sm udpateImageElement"
                alt=""
              />
            {:else}
              <img
                src={`${config.SERVER_IP}${config.SERVER_PORT}${image.address}`}
                class="shadow-sm udpateImageElement"
                alt=""
              />
            {/if}
            <input
              id="{index}_EditListingPopup"
              style="display: none;"
              type="file"
              bind:files={image.tempBlob}
              on:change={() => {
                let reader = new FileReader();
                reader.addEventListener("load", () => {
                  image.address = reader.result;
                });
                reader.readAsDataURL(image.tempBlob[0]);
              }}
            />
          </label>
        {/each}
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-sm btn-danger" on:click={toggleForEdit}>
        Close
      </button>
      <button class="btn btn-sm" on:click={updateListing}>Update</button>
    </div>
  </div>
</Modal>

<Modal backdrop={false} isOpen={openForAddNewRoom} toggle={toggleForAddNewRoom}>
  <div class="modal-header">
    Add new Room
    <button
      class="btn btn-close bg-white border-0"
      on:click={toggleForAddNewRoom}
    />
  </div>
  <div class="modal-body">
    {#each listingToBeEdited?.rooms as room, index}
      {#if !room.id}
        <Modal
          backdrop={true}
          isOpen={isOpenAmenitiesSheet}
          toggle={toggleAmenetiesSheet}
        >
          <div class="p-3">
            <Amenities
              selectedAmeneties={room.ameneties}
              amenitiesList={AllAmeneties}
              on:ameneties={(data) => {
                room.ameneties = data.detail.ameneties;
              }}
            />
          </div>
        </Modal>
        <div class="row mb-3">
          <div class="col-md-12">
            <input
              type="text"
              placeholder="Category Title"
              class="form-control mb-2"
              bind:value={room.RoomCategory}
            />
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            <input
              type="text"
              placeholder="Category Description"
              class="form-control mb-2"
              bind:value={room.RoomDescription}
            />
          </div>
        </div>
        <div class="row mb-2">
          <div class="col-12">
            <button
              class="btn btn-light btn-sm"
              on:click={() => (isOpenAmenitiesSheet = !isOpenAmenitiesSheet)}
            >
              Ameneities
            </button>
          </div>
        </div>
        <div class="row">
          <div class="col-md-4">
            <input
              type="number"
              placeholder="Quantity"
              class="form-control mb-2"
              bind:value={room.Quantity}
            />
          </div>
          <div class="col-md-4">
            <input
              type="number"
              placeholder="Cost"
              class="form-control mb-2"
              bind:value={room.Cost}
            />
          </div>
          <div class="col-md-4">
            <div class="d-flex align-items-center" style="height: 90%;">
              <input
                id="{room.RoomCategory}_visible"
                type="checkbox"
                class="me-2"
                bind:checked={room.Visible}
              />
              <label for="{room.RoomCategory}_visible"> Visible </label>
            </div>
          </div>

          <div class="col-md-4 mb-2">
            {#if room.Images.length === 0}
              <button
                class="btn btn-sm border"
                on:click={() => {
                  room.Images = [
                    {
                      address: "",
                    },
                  ];
                }}
              >
                Add Room Image
              </button>
            {/if}
          </div>
        </div>
        <div class="row">
          <div class="col-md-12">
            {#each room?.Images as image, index2}
              <label
                class="udateImageContainer"
                for="{index2}_forRoomAdd_${index}"
              >
                <input
                  id="{index2}_forRoomAdd_${index}"
                  style="display: none;"
                  type="file"
                  bind:files={image.tempBlob}
                  on:change={() => {
                    let reader = new FileReader();
                    reader.addEventListener("load", () => {
                      image.address = reader.result;
                    });
                    reader.readAsDataURL(image.tempBlob[0]);
                  }}
                />
                <img src={image.address} alt="" class="udpateImageElement" />
              </label>
            {/each}
          </div>
        </div>
        <button
          class="btn mb-2"
          on:click={() => {
            listingToBeEdited?.rooms.splice(index, 1);
            listingToBeEdited.rooms = [...listingToBeEdited?.rooms];
          }}
        >
          Remove
        </button>
        <br />
      {/if}
    {/each}
    <div class="text-end">
      <button
        class="btn"
        on:click={() => {
          listingToBeEdited.rooms = [
            ...listingToBeEdited?.rooms,
            {
              RoomCategory: "",
              RoomDescription: "",
              Quantity: 1,
              Cost: 1,
              DiscountedPrice: 0,
              Images: [],
              meta: [],
              amenities: [],
              Visible: true,
            },
          ];
        }}>Add Room</button
      >
    </div>
  </div>
  <div class="modal-footer">
    <button class="btn btn-sm btn-danger" on:click={toggleForAddNewRoom}>
      Close
    </button>
    <button class="btn btn-sm" on:click={addRoomToTheListing}>Update</button>
  </div>
</Modal>

<Modal isOpen={openForRoomsEdit} toggle={toggleForRoomsEdit}>
  <div class="modal-header">
    Edit Rooms
    <button
      class="btn btn-close bg-white border-0"
      on:click={toggleForRoomsEdit}
    />
  </div>
  <div class="modal-body">
    {#each listingToBeEdited?.rooms as room, ri}
      <div class="row">
        <div class="col-md-12">
          <input
            type="text"
            placeholder="Category Title"
            class="form-control mb-2"
            bind:value={room.RoomCategory}
          />
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <input
            type="text"
            placeholder="Category Description"
            class="form-control mb-2"
            bind:value={room.RoomDescription}
          />
        </div>
      </div>
      <div class="row">
        <div class="col-md-4">
          <input
            type="number"
            placeholder="Quantity"
            class="form-control mb-2"
            bind:value={room.Quantity}
          />
        </div>
        <div class="col-md-4">
          <input
            type="number"
            placeholder="Cost"
            class="form-control mb-2"
            bind:value={room.Cost}
          />
        </div>
        <div class="col-md-4">
          <div class="d-flex align-items-center" style="height: 90%;">
            <input
              id="{room.RoomCategory}_visible"
              type="checkbox"
              class="me-2"
              bind:checked={room.Visible}
            />
            <label for="{room.RoomCategory}_visible"> Visible </label>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          {#each room?.Images as image, i}
            <label for="{i}_forRoomEdit_${ri}" class="udateImageContainer">
              <input
                style="display: none;"
                id="{i}_forRoomEdit_${ri}"
                type="file"
                bind:files={image.tempBlob}
                on:change={() => {
                  let reader = new FileReader();
                  reader.addEventListener("load", () => {
                    image.address = reader.result;
                  });

                  reader.readAsDataURL(image.tempBlob[0]);
                }}
              />
              <div class="border">
                {#if image.address.substring(0, 4) === "data"}
                  <img
                    src={`${image.address}`}
                    alt=""
                    class="udpateImageElement col-12"
                  />
                {:else}
                  <img
                    src={`${config.SERVER_IP}${config.SERVER_PORT}${image.address}`}
                    alt=""
                    class="udpateImageElement col-12"
                  />
                {/if}
              </div>
            </label>
          {/each}
        </div>
      </div>
    {/each}
  </div>
  <div class="modal-footer">
    <button class="btn btn-sm btn-danger" on:click={toggleForRoomsEdit}>
      Close
    </button>
    <button class="btn btn-sm" on:click={onUpdateRoomsDetail}>Update</button>
  </div>
</Modal>

<Modal isOpen={openForBookings} toggle={toggleForBookings}>
  <div class="modal-header">{bookingsData.listing.headline} Bookings</div>
  <div class="modal-body">
    {#if bookingsData?.bookings?.length === 0}
      <div>No bookings yet</div>
    {/if}
    {#each bookingsData?.bookings as booking, index}
      <h5>
        Booking {booking.Booking_id} for {booking.Invoice_amount}
        {bookingsData.listing.currency}
      </h5>
      <hr />
      <table class="table table-bordered">
        <tbody>
          <tr><th>Customer Details</th><th /></tr>
          <tr>
            <th>Email</th>
            <td>{booking.Customer_Email}</td>
          </tr>
          <tr>
            <th>Telephone</th>
            <td>{booking.Customer_Tel}</td>
          </tr>
          <tr>
            <th>Persons</th>
            <td>{booking.Persons}</td>
          </tr>
          <tr>
            <th>Approved</th>
            <td>{booking.Approved}</td>
          </tr>
        </tbody>
      </table>
      <button
        class="m-1 btn btn-sm"
        on:click={() => {
          var requestOptions = {
            method: "PUT",
          };

          fetch(
            `${config.SERVER_IP}${config.SERVER_PORT}/bookings/approve/${booking.Booking_id}`,
            requestOptions
          )
            .then((response) => response.text())
            .then((result) => {
              openForBookings = false;
              loadData();
            })
            .catch((error) => error);
        }}>Approve</button
      >
      <button
        class="m-1 btn btn-sm"
        on:click={() => {
          var requestOptions = {
            method: "PUT",
          };

          fetch(
            `${config.SERVER_IP}${config.SERVER_PORT}/bookings/disapprove/${booking.Booking_id}`,
            requestOptions
          )
            .then((response) => response.text())
            .then((result) => {
              openForBookings = false;
              loadData();
            })
            .catch((error) => error);
        }}>Disapprove</button
      >
      <hr />
    {/each}
  </div>
  <div class="modal-footer">
    <button class="btn btn-sm" on:click={toggleForBookings}>Close</button>
  </div>
</Modal>

<style>
  .btn {
    border: 1px solid #9427f7;
    background-color: #9427f7;
    color: white;
  }

  .btn:nth-child(n):hover {
    background-color: #6801c8;
    color: white;
  }

  .udateImageContainer {
    width: 24%;
    margin: 0.5%;
  }

  .udpateImageElement {
    width: 100%;
    border-radius: 5pt;
    transition: 0.2s;
    border: 2px solid transparent;
    object-fit: contain;
  }

  .udpateImageElement:hover {
    opacity: 0.3;
    border: 2px solid #9427f7;
  }
</style>
