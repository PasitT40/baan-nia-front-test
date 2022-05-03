<template>
  <div class="text-center">
    <v-dialog v-model="dialog.show" persistent width="700px">
      <v-card>
        <v-row no-gutters class="pa-3">
          <v-col cols="12">Create</v-col>
          <v-col cols="12">
            <v-row class="pa-1">
              <v-col cols="5">
                <v-text-field
                  v-model="name"
                  label="name"
                  background-color="white"
                  hide-details=""
                  outlined
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="postCode"
                  label="Post Code"
                  type="number"
                  hide-spin-buttons
                  background-color="white"
                  hide-details=""
                  outlined
                ></v-text-field>
              </v-col>
              <v-col cols="3">
                <v-text-field
                  v-model="price"
                  label="Price"
                  type="number"
                  hide-spin-buttons
                  background-color="white"
                  hide-details=""
                  outlined
                ></v-text-field>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="12" class="pt-3">
            <v-textarea
              v-model="description"
              label="Description"
              background-color="white"
              hide-details=""
              outlined
            ></v-textarea>
          </v-col>
          <v-col cols="12" class="pt-3 mr-5 d-flex justify-center">
            <div class="mr-5">
              <v-btn
                @click="dialog.show = false"
                outlined
                color="#7C7E82"
                height="45px"
                width="164px"
              >
                CANCEL
              </v-btn>
            </div>
            <div>
              <v-btn
                @click="create()"
                color="#22BB66"
                height="45px"
                width="164px"
              >
                <span class="white--text"> CREATE</span>
              </v-btn>
            </div>
          </v-col>
        </v-row>
      </v-card>
    </v-dialog>
    <ResultModal :result="result" />
  </div>
</template>

<script>
import ResultModal from "./ResultModal.vue";
import axios from "axios";

export default {
  components: {
    ResultModal,
  },
  name: "ModalFrom",
  props: {
    dialog: {
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
      name: "",
      postCode: "",
      price: "",
      description: "",
    };
  },
  methods: {
    create() {
      if (this.url != "" && this.port != "") {
        axios
          .post(`${this.url}/home`, {
            name: this.name,
            desc: this.description,
            price: this.price,
            post_code: this.postCode,
          })
          .then((response) => {
            console.log("completed", response.data);
            this.name = "";
            this.postCode = "";
            this.price = "";
            this.description = "";
            this.dialog.show = false;
            this.result.show = true;
            this.result.type = "create";
          })
          .catch((error) => {
            console.log("not complete", error);
            this.name = "";
            this.postCode = "";
            this.price = "";
            this.description = "";
            this.dialog.show = false;
            this.result.show = true;
            this.result.type = "fail";
          });
      }
    },
  },
};
</script>