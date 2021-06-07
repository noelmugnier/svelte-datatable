<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let row;
</script>

{#each Object.keys(row) as cell}
  <slot cell={row[cell]} />
{/each}