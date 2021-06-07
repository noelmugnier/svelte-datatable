<script>
  import { getContext } from "svelte";
  import contextKey from "./table";

  const tableService = getContext(contextKey);

  const refreshIfRequired = () => {
    if (!$tableService.useApplyFilters)
      tableService.refresh();
  };
</script>

<tr class="headers">
  <slot />
</tr>

{#if $tableService.columnFilterable && $tableService.showColumnFilters}
  <tr class="headers-filters">
    {#each Object.keys($tableService.columns) as columnName}
      {#if $tableService.columns[columnName].visible}
        <td>
          {#if $tableService.columns[columnName].filter}
            {#if $tableService.columns[columnName].type === "int"}
              <input type="number" bind:value={$tableService.filters[$tableService.columns[columnName].field]}
                     on:change={refreshIfRequired} />
            {:else if $tableService.columns[columnName].type === "boolean"}
              <input type="checkbox" bind:checked={$tableService.filters[$tableService.columns[columnName].field]}
                     on:change={refreshIfRequired} />
            {:else if $tableService.columns[columnName].type === "select"}
              <select bind:value={$tableService.filters[$tableService.columns[columnName].field]}
                      on:change={refreshIfRequired}>
                {#each $tableService.columns[columnName].values as value}
                  <option value={value.value}>{value.label}</option>
                {/each}
              </select>
            {:else if $tableService.columns[columnName].type === "date" || $tableService.columns[columnName].type === "daterange"}
              <input type="date" bind:value={$tableService.filters[$tableService.columns[columnName].field]}
                     on:change={refreshIfRequired} />
            {:else if $tableService.columns[columnName].type === "range"}
              <input type="range" min={$tableService.columns[columnName].values.min}
                     max={$tableService.columns[columnName].values.max}
                     bind:value={$tableService.filters[$tableService.columns[columnName].field]}
                     on:change={refreshIfRequired} />
            {:else}
              <input type="text" bind:value={$tableService.filters[$tableService.columns[columnName].field]}
                     on:change={refreshIfRequired} />
            {/if}
          {/if}
        </td>
      {:else if $tableService.columns[columnName].default}
        <td></td>
      {/if}
    {/each}
  </tr>
{/if}