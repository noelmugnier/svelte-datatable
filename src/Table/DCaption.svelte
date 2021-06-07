<script>
  import { createEventDispatcher, getContext } from "svelte";
  import contextKey from "./table";

  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);

</script>

<caption>
  <slot />
</caption>