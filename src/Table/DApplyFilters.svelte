<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  export let icon, name = "Apply filters", classes;
  
  const applyFilters = () => {
    dispatch('apply', $tableService.filters);
    tableService.refresh();
  };
  
  onMount(() => {
    tableService.enableApplyFilters();
  });
</script>

<button class={classes} on:click={applyFilters}>{name}</button>