<template>
  <v-app>
    <data-table :columnDefs="columnDefs" :rowData="rowData">
      <template
        #default="{ search, dense, rowAmount, gridOptions, rowHeight, deleteRow, setRowHeight, setRowAmount }"
      >
        <toolbar
          :search="search"
          :dense="dense"
          :rowAmount="rowAmount"
          :columnDefs="columnDefs"
          :gridOptions="gridOptions"
          :rowHeight="rowHeight"
          @update:rowHeight="setRowHeight"
          @deleteRow="deleteRow"
          @update:rowAmount="setRowAmount"
        />
      </template>
    </data-table>
  </v-app>
</template>

<script>
import DataTable from "./components/DataTable";
import Toolbar from "./components/Toolbar";

export default {
  name: "App",

  components: {
    DataTable,
    Toolbar
  },
  data: () => ({
    columnDefs: [],
    rowData: [],
    columnFields: [],
    defaultColDef: null,
    dialog: false,
    fluid: true,
    gridOptions: {},
    gridApi: null,
    pagination: true,
    pageAmounts: [10, 50, 100],
    rowHeight: 48,
    search: "",
    visibleColumns: []
  }),

  beforeMount() {
    (this.columnDefs = [
      {
        headerName: "Servicio",
        field: "service",
        checkboxSelection: true
      },
      {
        headerName: "CRF",
        field: "clientCode"
      },
      {
        headerName: "Codigo",
        field: "price"
      },
      {
        headerName: "Codigo1",
        field: "price1"
      },
      {
        headerName: "Codigo2",
        field: "price2"
      }
    ]),
      (this.defaultColDef = {
        sortable: true,
        resizable: true,
        filter: true,
        editable: true
      });

    this.rowData = [
      { service: "Planta Piloto", clientCode: "330", price: 35000 },
      { service: "900", clientCode: "340", price: 32000 },
      { service: "C1", clientCode: "342", price: 71000 },
      { service: "C2", clientCode: "343", price: 52000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 },
      { service: "C3", clientCode: "344", price: 12000 }
    ];
  }
};
</script>


<style>
@import "../node_modules/ag-grid-community/dist/styles/ag-grid.css";
@import "../node_modules/ag-grid-community/dist/styles/ag-theme-material.css";
</style>