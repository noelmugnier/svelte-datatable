<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);

  onMount(() => {
    tableService.enableAdvancedFiltering();
  });
</script>

<tr class="advanced-filters"
    style="{!$tableService.showAdvancedFilters ? 'display:none' : 'background-color: gray'}">
  <th colspan={$tableService.columnsCount}>
    <slot />
  </th>
</tr>