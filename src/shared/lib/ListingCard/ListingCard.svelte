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
                style="object-fit: cover;object-position: center;height: 230pt;"
                width="100%"
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
            <!-- <div style="font-size: 17.5px;" class="fw-bold">
              {listing.headline.length > 40
                ? listing.headline.substring(0, 40) + "..."
                : listing.headline}
            </div> -->
            <!-- <div class="fw-bold" style="font-size: 14px; color: #172b4d">
              <i class="bi bi-geo-alt-fill main-color" />
              <span>{listing.address}</span>
            </div> -->
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
              <span style="color:#9c59df;"><Icon name="star-fill" /></span>
              <span>4.9</span>
              <span class="text-muted">(120)</span>
            </span>
            <span class="h4">
              <span>$181</span>
              <span class="fw-normal h5">/ night</span>
            </span>
          </div>
        </div>
      </div>
    </div>
  </Link>
{/if}
