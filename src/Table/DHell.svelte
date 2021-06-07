<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);

  export let sort = false, display = null, filter = false, field, type = null, values = null,
    visible = true, size = 1, sorted = null;

  const orderBy = () => {
    tableService.setOrderBy(field);
  };

  onMount(() => {
    if (filter && !$tableService.columnFilterable)
      tableService.enableColumnFiltering();

    if (sort && !$tableService.sortable)
      tableService.enableSorting();

    tableService.addColumn(field, display, type, filter, sort, values, visible, size);
    if (sorted)
      orderBy();
  });

</script>

{#if !field || !$tableService.columns[field] || $tableService.columns[field].visible}
  <th class="hcell">
    {#if display}
      <span class="hcell-name">
        {display}
      </span>
      {#if field && $tableService.columns[field] && $tableService.columns[field].sort}
        <button class="hcell-sort" on:click={orderBy}>
          Orderby {$tableService.orderBy.field === field ? $tableService.orderBy.direction : ''}</button>
      {/if}
    {:else}
      <slot />
    {/if}
  </th>
{/if}
