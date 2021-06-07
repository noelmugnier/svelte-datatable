<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  export let icon, name = "Advanced filters", classes;
  
  onMount(() => {
    tableService.enableAdvancedFiltering(false);
  });
</script>

<button class={classes} on:click={() => tableService.toggleAdvancedFilters()}>{name}</button>