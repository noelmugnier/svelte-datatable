<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);

  export let field;

  onMount(() => {
  });

  $: getLeftPosition = (selectable, expandable) => {
    let left = 0;
    left = selectable ? left + 30 : left;
    left = expandable ? left + 30 : left;

    return left;
  };

</script>

{#if !field || !$tableService.columns[field] || $tableService.columns[field].visible}
  <td>
    <slot />
  </td>
{/if}