<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  export let icon, name = "Column filters", classes;
  
  onMount(() => {
    tableService.enableColumnFiltering(false);
  });
</script>

<button class={classes} on:click={() => tableService.toggleColumnFilters()}>{name}</button>