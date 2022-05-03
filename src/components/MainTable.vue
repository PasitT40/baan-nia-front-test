<template>
  <v-row v-if="items != null" no-gutters class="header-padding">
    <v-col cols="12" class="d-flex justify-center">
      <v-data-table
        :sort-by="['id']"
        :sort-desc="[true]"
        style="width: 100%"
        :headers="headers"
        :items-per-page="5"
        :items="items"
        :footer-props="{
          'disable-items-per-page': true,
        }"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-col>
    <EditForm
      :edit="editDialog"
      @refreshData="updateparent"
      :url="url"
      :port="port"
    />
    <ResultModal :result="result" />
  </v-row>
</template>

<script>
import axios from "axios";
import EditForm from "./EditForm.vue";
import ResultModal from "./ResultModal.vue";

export default {
  components: {
    EditForm,
    ResultModal,
  },
  name: "MainTable",
  props: {
    items: {
      required: true,
    },
    url: {
      required: true,
    },
    port: {
      required: true,
    },
  },
  data() {
    return {
      result: { show: false, type: "" },
      editDialog: { show: false, data: null },
      headers: [
        {
          text: "ID",
          value: "id",
          width: "1%",
          align: "center",
        },
        { text: "Name", value: "name", align: "center" },
        { text: "Post Code", value: "desc", align: "center" },
        { text: "Price", value: "price", align: "center" },
        { text: "Action", value: "actions", sortable: false, align: "center" },
      ],
    };
  },
  methods: {
    deleteItem(item) {
      if (this.url != "" && this.port != "") {
        axios
          .delete(`${this.url}/home/${item.id}`)
          .then((response) => {
            this.$emit("refreshData", response);
            this.result.show = true;
            this.result.type = "delete";
          })
          .catch(function (error) {
            console.log("not complete", error);
            this.result.type = "fail";
          });
      }
    },
    updateparent(variable) {
      this.$emit("refreshData", variable);
    },
    editItem(item) {
      console.log(item);
      this.editDialog = { show: true, data: item };
    },
  },
};
</script>

<style>
</style>