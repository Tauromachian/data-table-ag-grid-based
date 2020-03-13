<template>
  <v-row>
    <v-col md="2">
      <v-btn color="primary" @click="deleteRow">Borrar columna</v-btn>
    </v-col>

    <v-col md="2">
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on }">
          <v-btn color="primary" v-on="on">Columnas</v-btn>
        </template>

        <v-card>
          <app-column-select-list :columns="columnDefs" v-model="visibleColumns" />

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">Aceptar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-col>

    <v-col md="1">
      <v-select v-model="rowAmount" :items="pageAmounts" label="Filas"></v-select>
    </v-col>

    <v-col md="4">
      <v-text-field
        v-model="search"
        label="Buscar"
        single-line
        hide-details
        append-icon="mdi-magnify"
      ></v-text-field>
    </v-col>
    <v-col md="1">
      <v-btn-toggle group mandatory v-model="rowHeight">
        <v-btn value="48">
          <v-icon>mdi-menu</v-icon>
        </v-btn>
        <v-btn value="24">
          <v-icon>mdi-format-align-justify</v-icon>
        </v-btn>
      </v-btn-toggle>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "Toolbar",
  data: () => {
    return {
      dialog: false
    };
  },
  props: {
    search: String,
    dense: Boolean,
    rowAmount: Number,
    columnDefs: Array
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
    }
  }
};
</script>