<template>
  <v-container fluid>
    <slot
      :search="search"
      :dense="dense"
      :rowAmount="rowAmount"
      :gridOptions="gridOptions"
      :rowHeight="rowHeight"
    ></slot>

    <ag-grid-vue
      style="width: inherit; height: 500px;"
      class="ag-theme-material"
      :defaultColDef="defaultColDef"
      :columnDefs="columnDefs"
      :rowData="rowData"
      :quickFilterText="search"
      :pagination="pagination"
      :paginationPageSize="rowAmount"
      :gridOptions="gridOptions"
    ></ag-grid-vue>
  </v-container>
</template>

<script>
export default {
  name: "DataTable",
  props: {
    defaultColDef: Object,
    columnDefs: {
      type: Array,
      required: true
    },
    rowData: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      dense: false,
      gridOptions: {},
      gridApi: {},
      pagination: true,
      rowAmount: 10,
      rowHeight: 48,
      search: ""
    };
  },

  methods: {
    setNoDense() {
      this.dense = false;
    },
    setDense() {
      this.dense = true;
    }
  },
  watch: {
    rowAmount: function(val) {
      this.gridApi.paginationSetPageSize(Number(val));
    },
    rowHeight: function(val) {
      this.gridOptions.rowHeight = Number(val);
      this.gridApi.resetRowHeights();
    },
    visibleColumns: function(val) {
      let difference = this.columnFields.filter(x => !val.includes(x));
      this.gridOptions.columnApi.setColumnsVisible(val, true);
      this.gridOptions.columnApi.setColumnsVisible(difference, false);
    }
  }
};
</script>