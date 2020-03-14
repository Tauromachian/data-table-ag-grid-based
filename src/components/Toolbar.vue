<template>
  <v-row>
    <v-col md="2">
      <v-btn @click="deleteRow">Borrar columna</v-btn>
    </v-col>

    <v-col md="2">
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on }">
          <v-btn v-on="on">Columnas</v-btn>
        </template>

        <v-card>
          <column-select-list :columns="columnDefs" v-model="visibleColumns" />

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn text @click="dialog = false">Aceptar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-col>

    <v-col md="1">
      <v-select v-model="setRowAmount" :items="pageAmounts" label="Filas"></v-select>
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
      <v-btn-toggle group mandatory v-model="setRowHeight">
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
import ColumnSelectList from "./ColumnSelectList";

export default {
  name: "Toolbar",
  components: {
    ColumnSelectList
  },
  data() {
    return {
      dialog: false,
      visibleColumns: [],
      pageAmounts: [10, 50, 100],
    };
  },
  props: {
    search: String,
    dense: Boolean,
    rowAmount: Number,
    columnDefs: Array,
    gridOptions: Object,
    rowHeight: String
  },

  computed: {
    setRowHeight:{
      get(){
        return this.rowHeight;
      },
      set(val){
        this.$emit("update:rowHeight", val);
      }
    },
    setRowAmount: {
      get(){
        return this.rowAmount;
      },
      set(val){
        this.$emit("update:rowAmount", Number(val));
      }
    }
  },

  methods: {
    deleteRow(){
      this.$emit("deleteRow");
    }
  }
};
</script>