<template>
  <v-container fluid>
    <slot :search="search" :dense="dense" :rowAmount="rowAmount"></slot>

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
    },
  },
  data() {
    return {
      dense: false,
      gridOptions: {},
      gridApi: {},
      pagination: true,
      rowAmount: 10,
      search: "",
    };
  },

  methods: {
    setNoDense() {
      this.dense = false;
    },
    setDense() {
      this.dense = true;
    }
  }
};
</script>