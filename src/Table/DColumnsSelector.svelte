<script>
  import { createEventDispatcher, getContext } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  export let icon, name = "Columns visibility", classes;

  const updateHiddenColumns = (e) => {
    tableService.setVisibleColumns(Array.from(e.target.selectedOptions).map(s => s.value));
  };
</script>

<button class={classes} on:click={() => tableService.toggleColumnsSelector()}>{name}</button>
{#if $tableService.showColumnSelector}
  <ul style="text-align: left">
    {#each Object.keys($tableService.columns) as columnField}
      {#if $tableService.columns[columnField].display}
        <li>
          <input type="checkbox"
                 bind:checked={$tableService.columns[columnField].visible} />{$tableService.columns[columnField].display}
        </li>
      {/if}
    {/each}
  </ul>
{/if}