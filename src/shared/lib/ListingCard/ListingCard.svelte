<script>
  import { Link } from "svelte-routing";
  import { Icon } from "sveltestrap";
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
                height="280pt"
              />
            {/if}
          {/if}
        </div>
        <div
          class="col-md-6 px-5 py-4 d-flex flex-column justify-content-between"
        >
          <div class="col-md-12" style="color: #434859;">
            <h3 class="fw-bolder" style="text-transform: capitalize;">
              {listing.headline.length > 40
                ? listing.headline.substring(0, 40) + "..."
                : listing.headline}
            </h3>
            <span>
              <i class="bi bi-geo-alt-fill main-color" />
              <span style="text-transform: capitalize;">{listing.address}</span>
            </span>
            <div class="mt-2">
              <span
                class="text-secondary"
                style="font-size: 15px;line-height:19px"
              >
                {listing.description.length > 200
                  ? listing.description.substring(0, 200) + "..."
                  : listing.description}
              </span>
              <span style="color: #9427F7;display: inline;"
                >{listing.description.length > 200 ? "read more" : ""}</span
              >
            </div>
          </div>

          <div class="text-secondary1 justify-content-between d-flex pe-2">
            <span style="font-size: 15px;">
              {#if listing?.rating}
                <span style="color:#9c59df;"><Icon name="star-fill" /></span>
                <span>{listing?.rating}</span>
                {#if listing?.num_reviews}
                  <span class="text-muted">({listing?.num_reviews})</span>
                {/if}
              {:else}
                <span class="bg-purple text-white p-1 px-2 rounded">New</span>
              {/if}
            </span>
            <span class="h4">
              {#if listing?.min_price}
                <span>${listing?.min_price}</span>
                <span class="fw-normal h5">/ night</span>
              {/if}
            </span>
          </div>
        </div>
      </div>
    </div>
  </Link>
{/if}
