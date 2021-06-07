<script>
  import { createEventDispatcher, getContext, onMount, tick } from "svelte";
  import contextKey from "./table";
  import rowKey from "./row";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  const { row } = getContext(rowKey);

  export let multi;

  const toggleExpandRow = async () => {
    tableService.expandRow(row);
    await tick();
    dispatch("expand", { identifier: row[$tableService.rowIdentifier], expanded: isRowExpanded });
  };

  onMount(() => {
    if (!$tableService.multiRowExpandable && multi)
      tableService.enableExpandable(multi);
  });

  $: isRowExpanded = $tableService.expandedRows.some(r => r === row[$tableService.rowIdentifier]);
</script>

<th class="fixed-expand" on:click|stopPropagation={toggleExpandRow}>
  {#if isRowExpanded}
    &darr;
  {:else}
    &rarr;
  {/if}
</th>