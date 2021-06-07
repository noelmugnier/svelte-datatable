<script>
  import { getContext, onMount } from "svelte";
  import contextKey from "./table";

  const tableService = getContext(contextKey);

  onMount(() => {
    tableService.enableSelectable();
  });

</script>

<th class="fixed-select">
  <input type="checkbox" on:input={(e) => tableService.selectAllRows(e.target.checked)}
         checked={$tableService.allSelected} />
</th>