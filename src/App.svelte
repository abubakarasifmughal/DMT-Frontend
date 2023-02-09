<script lang="ts">
  import { Router } from "svelte-routing";
  import { Styles } from "sveltestrap";
  import RounterOutlet from "./shared/lib/Router/RounterOutlet.svelte";

  let loginToken: string = "";
</script>

<div>
  <RounterOutlet {loginToken} />
</div>

<Styles />
