<template>
  <v-container fluid>
    <slot
      :search="search"
      :rowAmount="rowAmount"
      :gridOptions="gridOptions"
      :rowHeight="rowHeight"
      :visibleColumns="visibleColumns"
      :setVisibleColumns="changeVisibleColumns"
      :deleteRow="deleteRow"
      :setRowHeight="setRowHeight"
      :setRowAmount="setRowAmount"
    >
      <toolbar
        :search.sync="search"
        :rowAmount.sync="rowAmount"
        :columnDefs="columnDefs"
        :gridOptions="gridOptions"
        :rowHeight="rowHeight"
        :visibleColumns="visibleColumns"
        @update:visibleColumns="setVisibleColumns"
        @update:rowHeight="setRowHeight"
        @deleteRow="deleteRow"
      />
    </slot>

    <ag-grid-vue
      :style="style"
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
import Toolbar from "./Toolbar";
export default {
  name: "DataTable",

  components: {
    Toolbar
  },
  props: {
    width: {
      type: [Number, String],
      default: 'inherit'
    },
    height: {
      type: [Number, String],
      default: 500
    },
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
      gridOptions: {},
      gridApi: {},
      pagination: true,
      rowAmount: 10,
      rowHeight: "48",
      search: "",
      visibleColumns: [],
      columnFields: []
    };
  },
  computed:{
    style(){
      return `width: ${this.width}; height: ${this.height}px;`
    }
  },
  mounted() {
    this.gridApi = this.gridOptions.api;
    this.setVisibleColumns();
    this.setColumnFields();
  },
  methods: {
    setVisibleColumns: function() {
      this.visibleColumns = this.columnDefs.map(element => {
        if (this.gridOptions.columnApi.getColumn(element.field).visible) {
          return element.field;
        }
      });
    },
    changeVisibleColumns: function(val) {
      this.visibleColumns = val;
    },
    setColumnFields: function() {
      this.columnFields = this.columnDefs.map(element => {
        return element.field;
      });
    },
    deleteRow: function() {
      const selectedRow = this.gridApi.getFocusedCell();
      this.gridOptions.rowData.splice(selectedRow.rowIndex, 1);
      this.gridApi.setRowData(this.gridOptions.rowData);
    },
    setRowHeight: function(val) {
      this.rowHeight = val;
      this.gridOptions.rowHeight = Number(val);
      this.gridApi.resetRowHeights();
    },
    setRowAmount: function(val) {
      this.rowAmount = val;
    },
    doSearch: function (val) {
      this.search = val;
    }
  },
  watch: {
    rowAmount: function(val) {
      this.gridApi.paginationSetPageSize(Number(val));
    },

    visibleColumns: function(val) {
      let difference = this.columnFields.filter(x => !val.includes(x));
      this.gridOptions.columnApi.setColumnsVisible(val, true);
      this.gridOptions.columnApi.setColumnsVisible(difference, false);
    }
  }
};
</script>

<style lang="scss" scoped>
.ag-row {
  .ag-cell,
  .ag-cell .ag-icon {
    display: flex;
    align-items: center;
  }
}
</style>