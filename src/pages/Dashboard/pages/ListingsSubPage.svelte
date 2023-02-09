<script>
  import DashListings from "./DashListings.svelte";
  import EnListListings from "./EnListListings.svelte";
  let create = false;

  const onClickCreate = () => {
    create = true;
  };
</script>

<div>
  {#if create}
    <DashListings />
  {:else}
    <EnListListings {onClickCreate} />
  {/if}
</div>
