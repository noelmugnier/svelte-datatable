<script>
  import { getContext, onMount } from "svelte";
  import contextKey from "./table";

  const tableService = getContext(contextKey);

  export let multi = true;

  onMount(() => {
    tableService.enableExpandable(multi);
  });

</script>

<th class="fixed-expand">
  {#if $tableService.multiRowExpandable}
    <input type="checkbox" on:input={(e) => tableService.expandAllRows(e.target.checked)}
           checked={$tableService.allExpanded} />
  {/if}
</th>