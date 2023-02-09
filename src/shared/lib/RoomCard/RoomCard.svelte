<script>
  export let room;
  export let onClickAddtoCart;
  export let currency;
</script>

<div class="card shadow-sm p-3">
  <img
    src={room?.Images.length > 0 ? room?.Images[0]?.address : ""}
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
