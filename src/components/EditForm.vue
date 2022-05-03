<template>
  <div class="text-center">
    <v-dialog v-model="edit.show" persistent width="700px">
      <v-card>
        <v-row no-gutters class="pa-3">
          <v-col cols="12"
            >Item Detail -
            {{ edit.data != null ? edit.data.id : "null" }}</v-col
          >
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
                @click="edit.show = false"
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
                @click="update(edit.data.id)"
                color="#F6A623"
                height="45px"
                width="164px"
              >
                <span class="white--text"> UPDATE</span>
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
import axios from "axios";
import ResultModal from "./ResultModal.vue";

export default {
  name: "EditForm",
  components: {
    ResultModal,
  },
  props: {
    edit: {
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
  watch: {
    edit(val) {
      if (val.data != null) {
        this.name = val.data.name;
        this.postCode = val.data.post_code;
        this.price = val.data.price;
        this.description = val.data.desc;
      }
    },
  },
  methods: {
    update(id) {
      if (this.url != "" && this.port != "") {
        axios
          .patch(`${this.url}/home/${id}`, {
            name: this.name,
            desc: this.description,
            price: this.price,
            post_code: this.postCode,
          })
          .then((response) => {
            console.log("completed", response.data);
            this.$emit("refreshData", response);
            this.edit.show = false;
            this.result.show = true;
            this.result.type = "show";
          })
          .catch((error) => {
            console.log("not complete", error);
            this.result.show = true;
            this.result.type = "fail";
          });
      }
    },
  },
};
</script>