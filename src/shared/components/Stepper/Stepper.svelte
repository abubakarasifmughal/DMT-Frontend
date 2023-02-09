<script>
  export let title = '';
  export let steps = [];
  export let activeIndex = 0;
  export let substepIndex = 0;
</script>

<div style="width: 100%;">
  <div class="fw-bold" style="font-size: larger;">
    {title}
  </div>
  <div class="mt-4">
    {#each steps as step, index}
      <div>
        <div class="d-flex align-items-center">
          <div
            style="height: 15pt;width: 15pt;border-radius: 8pt;
            border: 2px solid {index <= activeIndex ? '#9427F7' : 'gainsboro'};
            background-color:{index <= activeIndex ? '#9427F7' : 'transparent'}"
          />
          <div class="p-2 mt-3 mb-3" style="font-size: medium;">{step.label}</div>
        </div>
        {#each step.substeps as substep,subIndex}
          <div class="m-4 ms-4 " style="font-size:medium;color:{subIndex<= substepIndex?'#9427F7':''}">{substep}</div>
        {/each}
      </div>
    {/each}
  </div>
</div>
