<script>
  import { Link } from "svelte-routing";

  export let listing;
</script>

<Link to="/listing/{listing.id}" class="text-decoration-none text-dark">
  <div class="card shadow mb-3" style="overflow: hidden;">
    {#if listing.listingImages.length > 0}
      <img
        src={listing.listingImages[0].address}
        alt=""
        style="object-fit: cover;object-position: center;"
        height="350px"
        width="100%"
      />
    {/if}
    <div class="p-4">
      <div class="fw-bold" style="font-size: x-large;color:#9427F7">
        {listing.headline}
      </div>
      <div class="text-secondary">{listing.address}</div>
      <div class="text-secondary" style="font-size: 15px;line-height:19px">
        {listing.description}
      </div>
      <!-- <br /> -->
      <!-- <div class="btn btn-light border">Check In</div> -->
    </div>
  </div>
</Link>
