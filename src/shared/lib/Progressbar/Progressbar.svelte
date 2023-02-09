<script lang="ts">
  export let total = 0;
  export let current = 0;
</script>

<div class="p-3" style="width: 90%;">
  <div class="container-fluid">
    <div class="mb-2">
        Progress
    </div>
    <div class="bgBar">
      <div style="width: {(current / total) * 100}%;" class="filled" />
    </div>
  </div>
</div>

<style>
  .bgBar {
    background-color: gainsboro;
    height: 10pt;
    border-radius: 50pt;
    width: 100%;
  }
  .filled {
    background-color: #9427f7;
    height: 10pt;
    border-radius: 50pt;
  }
</style>
