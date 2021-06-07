<script>
  import { createEventDispatcher, getContext, onMount } from "svelte";
  import contextKey from "./table";
  
  const dispatch = createEventDispatcher();
  const tableService = getContext(contextKey);
  
  export let showFirst = false, showLast = false, showPageNumbers = true, totalRows = null, page = 0, take = 10, takes = [10, 25, 50, 100];  
  let maxPage = null;
  
  const goTo = (page) => {
    tableService.goToPage(page);
  };
  
  const previous = () => {
    tableService.previousPage();
  }

  const next = () => {
    tableService.nextPage();
  }

  const range = (s, e) => {
    let x = [];
    while (s < e + 1) x.push(s++);
    return x;
  };
  
  onMount(() => {
    tableService.setPagination(page, take);
    maxPage = totalRows ? Math.floor(totalRows/take) : null;
  });  
</script>

<tr>
  <td colspan={$tableService.columnsCount}>
      <div>
        {#if showFirst}
          <button on:click={() => goTo(0)} disabled={$tableService.pagination.page < 1}>First</button>
        {/if}
        <button on:click={previous} disabled={$tableService.pagination.page < 1}>Previous</button>
        {#if showPageNumbers}
          {#if maxPage && maxPage > 10}
            {#each range(0, 2) as rPage}
              <button on:click={() => goTo(rPage)} disabled={rPage === $tableService.pagination.page}>{rPage + 1}</button>
            {/each}
            <button disabled="disabled">...</button>
            {#each range(tableService.pagination.page - 3, tableService.pagination.page + 3) as rPage}
              <button on:click={() => goTo(rPage)} disabled={rPage === $tableService.pagination.page}>{rPage + 1}</button>
            {/each}
            <button disabled="disabled">...</button>
            {#each range(maxPage - 3, maxPage) as rPage}
              <button on:click={() => goTo(rPage)} disabled={rPage === $tableService.pagination.page}>{rPage + 1}</button>
            {/each}
          {:else if maxPage}
            {#each range(0, maxPage) as rPage}
              <button on:click={() => goTo(rPage)} disabled={rPage === $tableService.pagination.page}>{rPage + 1}</button>
            {/each}
          {/if}
        {/if}
        <button on:click={next} disabled={maxPage ? $tableService.pagination.page >= maxPage : false}>Next</button>
        {#if showLast}
          <button on:click={() => goTo(maxPage)} disabled={$tableService.pagination.page >= maxPage}>Last</button>
        {/if}
      </div>
  </td>
</tr>