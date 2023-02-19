<script lang="ts">
  import { Router } from "svelte-routing";
  import { Styles } from "sveltestrap";
  import RounterOutlet from "./shared/lib/Router/RounterOutlet.svelte";

  let loginToken: string = "";
</script>

<div style="width: 100%;overflow:hidden">
  <RounterOutlet {loginToken} />
</div>

<Styles />
