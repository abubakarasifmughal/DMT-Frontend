<script>
  import DashListings from "./DashListings.svelte";
  import EnListListings from "./EnListListings.svelte";
  let create = false;

  const onClickCreate = () => {
    create = true;
  };

  const hideCreationUI = () => {
    create = false;
  };
</script>

<div>
  {#if create}
    <DashListings {hideCreationUI} />
  {:else}
    <EnListListings {onClickCreate} />
  {/if}
</div>
