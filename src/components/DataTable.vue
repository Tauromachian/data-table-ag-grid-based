<template>
  <v-container fluid>
    <slot
      :search="search"
      :dense="dense"
      :rowAmount="rowAmount"
      :gridOptions="gridOptions"
      :rowHeight="rowHeight"
      :deleteRow="deleteRow"
      :setRowHeight="setRowHeight"
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
    setVisibleColumns: function() {
      this.visibleColumns = this.columnDefs.map(element => {
        if (this.gridOptions.columnApi.getColumn(element.field).visible) {
          return element.field;
        }
      });
    },
    setColumnFields: function() {
      this.columnFields = this.columnDefs.map(element => {
        return element.field;
      });
    },
    deleteRow: function() {
      const selectedRow = this.gridApi.getFocusedCell();
      this.gridOptions.rowData.splice(selectedRow.index, 1);
      this.gridApi.setRowData(this.gridOptions.rowData);
    },
    setRowHeight: function (val) {
      this.rowHeight = val;
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