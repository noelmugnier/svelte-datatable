<script>
  import { createEventDispatcher, getContext, onMount, tick } from "svelte";
  import contextKey from "./table";
  import rowKey from "./row";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  const {row} = getContext(rowKey);

  const toggleSelectRow = async () => {
    tableService.selectRow(row);
    await tick();
    dispatch("select", { identifier: row[$tableService.rowIdentifier], selected: isRowSelected });
  };
  
  onMount(() => {
    tableService.enableSelectable();
  });
  
  $: isRowSelected = $tableService.selectedRows.some(r => r === row[$tableService.rowIdentifier]);
</script>

<th class="fixed-select" on:click|stopPropagation={toggleSelectRow}>
  <input type="checkbox" checked={isRowSelected} />
</th>