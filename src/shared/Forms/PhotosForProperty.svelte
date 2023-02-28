<script>
  import { createEventDispatcher } from "svelte";
  let dispatch = createEventDispatcher();
  export let onClickBack;
  export let onClickNext;
  let files;
  export let images = [];
</script>

<div class="bg-white shadow p-5 col-md-11">
  <h3>Add photos of your property</h3>
  <span>
    Show guests why they should pick your property with well-lit,
    landscape-oriented photos. Add at least 6 photos to publish your listing.
  </span>
  <hr />
  <br />
  <div class="col-md-12">
    <div class="col-12 p-5 dragInRegion text-center">
      <div>Drag and drop you photos here</div>
      <input
        type="file"
        multiple
        bind:files
        on:change={(data) => {
          images = [];
          for (let index = 0; index < files.length && index < 5; index++) {
            const element = files[index];
            let reader = new FileReader();
            reader.addEventListener("load", () => {
              images = [...images, { address: reader.result }];
            });
            reader.readAsDataURL(element);
          }
        }}
        id="listingPhotoAdder"
        style="visibility: hidden;position: absolute;"
      />
      <label for="listingPhotoAdder" class="btn mt-4">Upload Photos</label>
    </div>
    <div class="d-flex flex-wrap">
      {#each images as image, imind}
        <div class="btn-group me-3 mt-3" style="overflow: hidden;">
          <button
            class="btn btn-light border"
            on:click={() => {
              images.splice(imind, 1);
              images = images;
            }}
          >
            <div class="btn btn-close" />
          </button>
          <div
            id="abc"
            class="photo border-top border-end border-bottom"
            style="overflow: hidden;background-image: url({image.address})"
          />
        </div>
      {/each}
    </div>
  </div>
  <br />
  <br />
  <div class="mt-3 d-flex justify-content-between">
    <button
      on:click={() => {
        dispatch("listingImages", {
          images: images,
        });
        onClickBack();
      }}
      class="btn btn-light border pe-5 ps-5">Back</button
    >
    {#if onClickNext}
      <button
        on:click={() => {
          dispatch("listingImages", {
            images: images,
          });
          onClickNext();
        }}
        class="btn btn-light border pe-5 ps-5">Go Live</button
      >
    {/if}
  </div>
</div>

<style>
  .dragInRegion {
    border: 1.5px dashed gainsboro;
  }

  .btn {
    border: 1px solid #9427f7;
    background-color: #9427f7;
    color: white;
  }
  .btn:nth-child(n):hover {
    background-color: #6801c8;
    color: white;
  }
  .photo {
    height: 50pt;
    width: 50pt;
    background-color: gainsboro;
    border-top-right-radius: 5pt;
    border-bottom-right-radius: 5pt;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
  }
</style>
