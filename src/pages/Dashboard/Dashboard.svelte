<script>
  import { onMount } from "svelte";
  import { Link } from "svelte-routing";
  import { globalHistory } from "svelte-routing/src/history";
  import Navbar2 from "../../shared/components/Navbar2/Navbar2.svelte";
  import Amenities from "../../shared/Forms/Amenities.svelte";
  import AmenitiesManager from "./pages/AmenitiesManager.svelte";
  import FeaturedAds from "./pages/FeaturedAds.svelte";
  import ListingsSubPage from "./pages/ListingsSubPage.svelte";
  import ReservationsManager from "./pages/ReservationsManager.svelte";
  export let subpage;
  subpage;
  export let location;
  location;
  let pathname = window.location.pathname;
  let unsub;

  onMount(() => {
    pathname = location.pathname.split("/").reverse()[0];
    unsub = globalHistory.listen(({ location, action }) => {
      pathname = location.pathname.split("/").reverse()[0];
    });
  });

  let amenities = [
    {
      label: "Title",
      description: `Description`,
      options: [
        {
          label: "24 Hour Reception",
          checked: false,
          id: 1,
        },
        {
          label: "12 Hour Reception",
          checked: false,
          id: 1,
        },
      ],
    },
    {
      label: "Title 2",
      description: `Description 2`,
      options: [
        {
          label: "22 Hour Reception",
          checked: false,
          id: 1,
        },
        {
          label: "24 Hour Reception",
          checked: false,
          id: 1,
        },
      ],
    },
  ];
</script>

<div style="overflow-x: hidden;width: 100vw;">
  <Navbar2 color={"text-dark"} />
  <div class="row border-top">
    <div class="col-md-2 " style="font-size: large;">
      <Link to="/dashboard/home" style="text-decoration:none">
        <div class="col-12  text-dark sideBarTabs p-3 border-bottom text-start">
          Home
        </div>
      </Link>
      <Link to="/dashboard/inbox" style="text-decoration:none">
        <div class="col-12 text-dark sideBarTabs p-3 border-bottom text-start">
          Inbox
        </div>
      </Link>
      <Link to="/dashboard/listings" style="text-decoration:none">
        <div class="col-12 text-dark sideBarTabs p-3 border-bottom text-start">
          Listings
        </div>
      </Link>
      <Link to="/dashboard/reservations" style="text-decoration:none">
        <div class="col-12 text-dark sideBarTabs p-3 border-bottom text-start">
          Reservations
        </div>
      </Link>
      <Link to="/dashboard/featured" style="text-decoration:none">
        <div class="col-12 text-dark sideBarTabs p-3 border-bottom text-start">
          Featured
        </div>
      </Link>
      <Link to="/dashboard/amenities" style="text-decoration:none">
        <div class="col-12 text-dark sideBarTabs p-3 border-bottom text-start">
          Amenities
        </div>
      </Link>
    </div>
    <div
      class="col-md-10 bg-light border-start border-bottom"
      style="min-height: 90vh;"
    >
      {#if pathname === "home"}
        <div class="text-center p-5">
          <h3>Dashboards</h3>
        </div>
      {:else if pathname === "inbox"}
        <div class="text-center p-5">
          <h3>No Messages Yet</h3>
          <span>All your messages will appear here</span>
        </div>
      {:else if pathname === "listings"}
        <ListingsSubPage />
      {:else if pathname === "reservations"}
        <ReservationsManager />
      {:else if pathname === "featured"}
        <FeaturedAds />
      {:else if pathname === "amenities"}
        <AmenitiesManager {amenities} />
      {/if}
      <div style="height: 20vh;" />
    </div>
  </div>
</div>

<style>
  .sideBarTabs {
    border-left: 4pt solid transparent;
    cursor: pointer;
    display: block;
    transition: 0.2s;
    text-decoration: none;
  }
  .sideBarTabs:hover {
    border-left: 4pt solid #9427f7;
  }
</style>
