<!-- svelte-ignore a11y-click-events-have-key-events -->
<script>
  import { Modal } from "sveltestrap";
  import config from "../../../../environment.json";
  export let room;
  export let onClickAddtoCart;
  export let currency;

  let expandedImageUrl = "";
</script>

<div class="card shadow-sm p-3">
  <img
    on:click={() => (expandedImageUrl = room?.Images[0]?.address)}
    src={room?.Images.length > 0
      ? config.SERVER_IP + config.SERVER_PORT + room?.Images[0]?.address
      : ""}
    height="250px"
    style="object-fit: cover;object-position: top;"
    alt=""
  />
  <div class="mt-3">
    <h3>{room.RoomCategory}</h3>
    <div>{room.RoomDescription}</div>
  </div>
  <hr />
  <button class="btn btn-light border" on:click={onClickAddtoCart}>
    Reserve for {room.Cost}
    {currency} /night
  </button>
</div>

<Modal
  size="xl"
  centered
  isOpen={expandedImageUrl !== ""}
  style="position:absolute;z-index: 3000;"
>
  <div class="modal-header">
    {expandedImageUrl}
    <button class="btn btn-close" on:click={() => (expandedImageUrl = "")} />
  </div>
  <div class="modal-body overflow-hidden">
    <img
      src={config.SERVER_IP + config.SERVER_PORT + expandedImageUrl}
      alt=""
      style="width: 100%;object-fit:contain"
    />
  </div>
</Modal>
