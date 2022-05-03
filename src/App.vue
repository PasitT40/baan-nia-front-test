<template>
  <v-app>
    <v-container fluid class="pa-0">
      <v-row no-gutters class="pt-15 pb-6 header-padding bg-main">
        <v-col cols="12">
          <v-row no-gutters>
            <v-col cols="5">
              <v-row no-gutters>
                <v-col cols="12">URL</v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="url"
                    background-color="white"
                    hide-details=""
                    outlined
                  ></v-text-field
                ></v-col>
              </v-row>
            </v-col>
            <v-col cols="5">
              <v-row no-gutters class="px-15">
                <v-col cols="12">PORT</v-col>
                <v-col cols="12">
                  <v-text-field
                    type="number"
                    background-color="white"
                    hide-spin-buttons
                    v-model="port"
                    hide-details=""
                    outlined
                  ></v-text-field
                ></v-col>
              </v-row>
            </v-col>
            <v-col cols="2" class="d-flex align-end">
              <v-btn
                @click="loadData()"
                depressed
                color="#3C64B1"
                width="200px"
                height="53px"
              >
                <span class="white--text text-subtitle-1">Connect</span>
              </v-btn>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
      <v-row v-if="items != null" no-gutters class="header-padding">
        <v-col cols="6" class="d-flex align-center py-5">
          <span class="text-h5 font-weight-medium">House List</span>
        </v-col>
        <v-col cols="6" class="d-flex justify-end align-center py-5">
          <v-btn
            @click="dialog.show = true"
            depressed
            color="#22BB66"
            width="200px"
            height="53px"
          >
            <span class="white--text text-subtitle-1">CREATE</span>
          </v-btn>
        </v-col>
      </v-row>
      <MainTable
        :items="items"
        @refreshData="loadData"
        :url="url"
        :port="port"
      />
      <PostSelect :posts="posts" :url="url" :port="port" />
      <ModalForm :dialog="dialog" :url="url" :port="port" />
      <ResultModal :result="result" />
    </v-container>
  </v-app>
</template>

<script>
import MainTable from "./components/MainTable.vue";
import PostSelect from "./components/PostSelect.vue";
import ModalForm from "./components/ModalForm.vue";
import ResultModal from "./components/ResultModal.vue";
import axios from "axios";

export default {
  components: {
    MainTable,
    PostSelect,
    ModalForm,
    ResultModal,
  },
  data() {
    return {
      url: "",
      port: "",
      items: null,
      posts: null,
      dialog: { show: false },
      result: {show:false}
    };
  },
  methods: {
    loadData() {
      if (this.url != "" && this.port != "") {
        axios
          .get(`${this.url}/home`, { port: this.port })
          .then((response) => {
            console.log(response.data);
            this.items = response.data.payload;
          })
          .catch((e) => {
            this.errors.push(e);
          });
      }
      this.loadPost();
    },
    loadPost() {
      if (this.url != "" && this.port != "") {
        axios
          .get(`${this.url}/postCode`, { port: this.port })
          .then((response) => {
            console.log(response.data.payload);
            this.posts = response.data.payload;
          });
      }
    },
  },
};
</script>

<style lang="scss">
.header-padding {
  padding-left: 150px;
  padding-right: 150px;
}

.bg-main {
  background-color: #f4f7fc;
}

th {
  border: solid 1px #e0e0e0;
}
td {
  border: solid 1px #e0e0e0;
}
</style>