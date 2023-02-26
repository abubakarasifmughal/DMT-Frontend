<script>
  import { Link } from "svelte-routing";
  import config from "../../../../environment.json";

  export let listing;
</script>

{#if listing.headline !== ""}
  <Link to="/listing/{listing.id}" class="text-decoration-none text-dark">
    <div class="card shadow mb-3" style="overflow: hidden;">
      <div class="row">
        <div class="col-md-6">
          {#if listing?.listingImages?.length > 0}
            {#if listing.listingImages[0]?.address !== undefined && listing.listingImages[0]?.address !== ""}
              <img
                src={`${config.SERVER_IP}${config.SERVER_PORT}${listing.listingImages[0]?.address}`}
                alt=""
                style="object-fit: cover;object-position: center;"
                width="100%"
                height="100%"
              />
            {/if}
          {/if}
        </div>
        <div class="col-md-6 p-3">
          <div class="fw-bold" style="font-size: x-large;color:#9427F7">
            {listing.headline}
          </div>
          <div class="text-secondary">{listing.address}</div>
          <span class="text-secondary" style="font-size: 15px;line-height:19px">
            {listing.description.length > 300
              ? listing.description.substring(0, 300) + " ..."
              : listing.description}
          </span>
          <span style="color: #9427F7;display: inline;"
            >{listing.description.length > 300 ? "read more" : ""}</span
          >
        </div>
      </div>
    </div>
  </Link>
{/if}
