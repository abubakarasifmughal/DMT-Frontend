<script>
  let strength = [1, 2, 3, 4, 5, 6];
  export let password = "";
  $: strengthIndex = password.length/2;
</script>

<div class="mb-3 row">
  {#each strength as bar, index}
    <div class="col-2">
      <div
        style="height: 8pt;border-radius: 5pt;background-color: {index >=
        strengthIndex
          ? 'gainsboro'
          : '#9427F7'};"
      />
    </div>
  {/each}
</div>

<style></style>
