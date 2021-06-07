<script>
  import { createEventDispatcher, onMount, setContext } from "svelte";
  import contextKey from "./table";
  import { writable } from "svelte/store";

  const dispatch = createEventDispatcher();
  export let instance = null;

  const store = writable({
    columns: {},
    rows: [],
    selectedRows: [],
    expandedRows: [],
    filters: {},
    pagination: {},
    showColumnSelector: false,
    showColumnFilters: false,
    showAdvancedFilters: false,
    selectable: false,
    rowIdentifier: null,
    allSelected: false,
    allExpanded: false,
    columnsCount: 0,
    expandable: false,
    sortable: false,
    orderBy: { field: null, direction: "NONE" },
    multiRowExpandable: false,
    advancedFilterable: false,
    columnFilterable: false,
    useApplyFilters: false
  });

  const refreshTable = () => {
    let hasFilters = false;
    if (Object.keys($store.filters).find(f => $store.filters[f]))
      hasFilters = true;

    let hasOrderBy = false;
    if ($store.orderBy.field)
      hasOrderBy = true;

    dispatch("refresh", {
      filters: hasFilters ? $store.filters : null,
      pagination: $store.pagination,
      orderBy: hasOrderBy ? $store.orderBy : null
    });
  };

  const tableService = {
    ...store,
    setRows: (rows) => {
      store.update(s => {
        if ($store.pagination.page != null)
          s.rows = rows;
        else {
          let localRows = s.rows;
          for (let i = 0; i < rows.length; i++)
            localRows.push(rows[i]);

          s.rows = localRows;
        }
        return s;
      });
    },
    setRowIdentifier: (rowIdentifier) => {
      store.update(s => {
        s.rowIdentifier = rowIdentifier;
        return s;
      });
    },
    enableApplyFilters: () => {
      store.update(s => {
        s.useApplyFilters = true;
        return s;
      });
    },
    resetPagination: () => {
      store.update(s => {
        if ($store.pagination.page != null)
          s.pagination.page = 0;
        else 
          s.pagination.cursor = null;
        
        s.rows = [];
        s.expandedRows = [];
        s.selectedRows = [];
        return s;
      });
      
      refreshTable();
    },
    refresh: () => {
      refreshTable();
    },
    setOrderBy: (field) => {
      store.update(s => {
        if (s.orderBy.field === field && s.orderBy.direction === "ASC") {
          s.orderBy.field = field;
          s.orderBy.direction = "DESC";
        } else if (s.orderBy.field === field && s.orderBy.direction === "DESC") {
          s.orderBy.field = null;
          s.orderBy.direction = "NONE";
        } else {
          s.orderBy.field = field;
          s.orderBy.direction = "ASC";
        }

        s.expandedRows = [];
        s.selectedRows = [];
        s.rows = [];
        
        if(s.pagination.page != null)
          s.pagination.page = 0;

        if(s.pagination.cursor != null)
          s.pagination.cursor = null;

        return s;
      });

      refreshTable();
    },
    clearFilters: () => {
      store.update(s => {
        Object.keys(s.filters).map(f => {
          s.filters[f] = null;
          return f;
        });

        return s;
      });
    },
    enableAdvancedFiltering: (showAdvancedFilters) => {
      store.update(s => {
        if (s.advancedFilterable)
          return s;

        s.advancedFilterable = true;
        s.showAdvancedFilters = showAdvancedFilters ?? true;
        return s;
      });
    },
    enableColumnFiltering: (showColumnFilters) => {
      store.update(s => {
        if (s.columnFilterable)
          return s;

        s.columnFilterable = true;
        s.showColumnFilters = showColumnFilters ?? true;
        return s;
      });
    },
    enableSorting: () => {
      store.update(s => {
        s.sortable = true;
        return s;
      });
    },
    enableExpandable: (multi) => {
      store.update(s => {
        if (!s.expandable) {
          let column = {
            field: "expand",
            type: null,
            display: null,
            position: Object.keys(s.columns).length,
            filter: false,
            sort: false,
            values: null,
            visible: false,
            size: 1,
            default: true
          };

          s.columns[column.field] = column;
          s.columnsCount = Object.keys(s.columns).reduce((prev, item) => prev + s.columns[item].size, 0);
        }

        s.expandable = true;
        s.multiRowExpandable = multi;
        return s;
      });
    },
    enableSelectable: () => {
      store.update(s => {
        if (!s.selectable) {
          let column = {
            field: "select",
            type: null,
            display: null,
            position: Object.keys(s.columns).length,
            filter: false,
            sort: false,
            values: null,
            visible: false,
            size: 1,
            default: true
          };

          s.columns[column.field] = column;
          s.columnsCount = Object.keys(s.columns).reduce((prev, item) => prev + s.columns[item].size, 0);
        }

        s.selectable = true;
        return s;
      });
    },
    addColumn: (field, display, type, filter, sort, values, visible, size) => {
      store.update(s => {
        if (s.columns[field])
          throw "Duplicate header field attributes found";

        let column = {
          field: field,
          display: display,
          type: type,
          position: Object.keys(s.columns).length,
          filter: filter,
          sort: sort,
          values: values,
          visible: visible,
          size: size,
          default: false
        };

        s.columns[column.field] = column;
        s.columnsCount = Object.keys(s.columns).reduce((prev, item) => prev + s.columns[item].size, 0);

        if (filter) {
          s.filters[field] = null;
          s.columnFilterable = true;
        }

        return s;
      });
    },
    addFilter: (field) => {
      store.update(s => {
        s.filters[field] = null;
        return s;
      });
    },
    toggleColumnFilters: () => {
      store.update(s => {
        s.showColumnFilters = !s.showColumnFilters;
        return s;
      });
    },
    toggleColumnsSelector: () => {
      store.update(s => {
        s.showColumnSelector = !s.showColumnSelector;
        return s;
      });
    },
    setVisibleColumns: (columns) => {
      store.update(s => {
        s.columns.map(c => {
          if (!c.type)
            return;

          c.visible = !columns.some(col => col === c.field);
        });

        return s;
      });
    },
    setPagination: (page, take) => {
      store.update(s => {
        s.pagination.page = page;
        s.pagination.take = take;

        return s;
      });

      refreshTable();
    },
    setInfinite: (cursor, take) => {
      store.update(s => {
        s.pagination.cursor = cursor;
        s.pagination.take = take;

        return s;
      });

      refreshTable();
    },
    nextPage: () => {
      store.update(s => {
        s.pagination.page++;
        s.expandedRows = [];
        s.selectedRows = [];
        return s;
      });

      refreshTable();
    },
    previousPage: () => {
      store.update(s => {
        s.pagination.page--;
        s.expandedRows = [];
        s.selectedRows = [];
        return s;
      });

      refreshTable();
    },
    goToPage: (page) => {
      store.update(s => {
        s.pagination.page = page;
        s.expandedRows = [];
        s.selectedRows = [];
        return s;
      });

      refreshTable();
    },
    loadMore: (cursor) => {
      store.update(s => {
        s.pagination.cursor = cursor;
        return s;
      });

      refreshTable();
    },
    toggleAdvancedFilters: () => {
      store.update(s => {
        s.showAdvancedFilters = !s.showAdvancedFilters;
        return s;
      });
    },
    expandRow: row => {
      if (!$store.multiRowExpandable) {
        if ($store.expandedRows.find(r => r === row[$store.rowIdentifier]) != null)
          store.update(s => {
            s.expandedRows = [];
            s.allExpanded = false;
            return s;
          });
        else
          store.update(s => {
            s.expandedRows = [row[$store.rowIdentifier]];
            s.allExpanded = false;
            return s;
          });
      } else {
        if ($store.expandedRows.find(r => r === row[$store.rowIdentifier]) != null)
          store.update(s => {
            s.expandedRows = s.expandedRows.filter(r => r !== row[$store.rowIdentifier]);
            s.allExpanded = s.expandedRows.length === s.rows.length;
            return s;
          });
        else
          store.update(s => {
            s.expandedRows = [...s.expandedRows, row[$store.rowIdentifier]];
            s.allExpanded = s.expandedRows.length === s.rows.length;
            return s;
          });
      }
    },
    expandAllRows: (expanded) => {
      if (!expanded)
        store.update(s => {
          s.expandedRows = [];
          s.allExpanded = false;
          return s;
        });
      else {
        store.update(s => {
          s.expandedRows = s.rows.map(r => r[$store.rowIdentifier]);
          s.allExpanded = true;
          return s;
        });
      }
    },
    selectAllRows: (selected) => {
      if (!selected)
        store.update(s => {
          s.selectedRows = [];
          s.allSelected = false;
          return s;
        });
      else {
        store.update(s => {
          s.selectedRows = s.rows.map(r => r[$store.rowIdentifier]);
          s.allSelected = true;
          return s;
        });
      }
    },
    selectRow: row => {
      if ($store.selectedRows.find(r => r === row[$store.rowIdentifier]) != null)
        store.update(s => {
          s.selectedRows = s.selectedRows.filter(r => r !== row[$store.rowIdentifier]);
          s.allSelected = s.selectedRows.length === s.rows.length;
          return s;
        });
      else
        store.update(s => {
          s.selectedRows = [...s.selectedRows, row[$store.rowIdentifier]];
          s.allSelected = s.selectedRows.length === s.rows.length;
          return s;
        });
    }
  };

  setContext(contextKey, tableService);

  onMount(() => {
    instance = {
      updateFilters: (field, value) => {
        store.update(s => {
          s.filters[field] = value;

          if (!$store.useApplyFilters)
            tableService.refresh();

          return s;
        });
      }
    };
  });
</script>

<table>
  <slot />
</table>