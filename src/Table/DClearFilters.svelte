<script>
  import { createEventDispatcher, getContext } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  export let icon, name = "Clear filters", classes;
  
  const clearFilters = () => {
    tableService.clearFilters();
    dispatch('clear', $tableService.filters);
  }
</script>

<button class={classes} on:click={clearFilters}>{name}</button>