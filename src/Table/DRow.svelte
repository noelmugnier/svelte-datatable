<script>
  import { getContext, setContext } from "svelte";
  import contextKey from "./table";
  import rowKey from "./row";

  const tableService = getContext(contextKey);
  export let row;

  setContext(rowKey, {row});
</script>

<tr id="{row[$tableService.rowIdentifier]}" on:click|stopPropagation>
  <slot />
</tr>