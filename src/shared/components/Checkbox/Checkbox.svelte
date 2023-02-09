<script lang="ts">
  export let name: string;
  export let checked: boolean = false;
</script>

<div class="d-flex gap-2 w-100 checkbox p-4 align-items-center">
  <input bind:checked type="checkbox" class="form-check-input __input" />
  <span class="form-check-label">{name}</span>
</div>

<style>
  .checkbox {
    width: 100%;
    height: 75pt;
    background-color: #f4f4f4;
    border-left: solid #f0f0f0 6px;
    border-right: solid #f0f0f0 6px;
  }

  .__input {
    width: 16px;
    height: 16px;
  }

  span {
    font-size: var(--text-h5);
    text-transform: capitalize;
  }
</style>