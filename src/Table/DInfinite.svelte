<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";
  
  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  
  export let totalRows = null, cursor = null, take = 10, takes = [10, 25, 50, 100];  
  
  const more = () => {
    tableService.loadMore(cursor);
  };
  
  const reset = () => {
    tableService.resetPagination();
  };
  
  onMount(() => {
    tableService.setInfinite(cursor, take);
  });  
</script>

<tr>
  <td colspan={$tableService.columnsCount}>
      <button on:click={reset}>Reset</button>
      <button on:click={more}>Load more</button>
  </td>
</tr>