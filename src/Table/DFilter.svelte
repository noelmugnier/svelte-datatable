<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);

  export let field, type = null, values = null, hidden = false;

  onMount(() => {
    tableService.addFilter(field);
  });
</script>

{#if !hidden}
  <slot />
{/if}

<!--TODO add default input if no slot-->