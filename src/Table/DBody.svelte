<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();

  export let rows = [], rowIdentifier;

  const tableService = getContext(contextKey);

  const assignRows = (newRows) => {
    console.log(newRows);
    tableService.setRows(newRows);
  };

  $: assignRows(rows);

  onMount(() => {
    tableService.setRowIdentifier(rowIdentifier);
  });

</script>

<tbody>
{#each $tableService.rows as row}
  <slot {row} />
{/each}
</tbody>