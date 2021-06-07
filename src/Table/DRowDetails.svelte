<script>
  import { getContext, onMount } from "svelte";
  import contextKey from "./table";

  export let row;

  const tableService = getContext(contextKey);

  onMount(() => {
  });

  $: isRowExpanded = $tableService.expandedRows.some(r => r === row[$tableService.rowIdentifier]);
</script>

{#if isRowExpanded}
  <tr id="{row[$tableService.rowIdentifier]}-details">
    <td colspan={$tableService.columnsCount}>
      <slot />
    </td>
  </tr>
{/if}
