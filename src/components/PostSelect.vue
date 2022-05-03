<template>
  <v-row
    v-if="posts != null"
    align="center"
    justify="center"
    no-gutters
    class="bg-main margin-main"
  >
    <v-col cols="4" class="d-flex justify-center">
      <v-row no-gutters>
        <v-col cols="12">
          <v-select
            background-color="white"
            v-model="selectedPost"
            label="SELECT POST CODE"
            :items="posts"
            outlined
            hide-details=""
            item-text="post_code"
            @change="loadDetail()"
          ></v-select>
        </v-col>
        <v-col v-if="data.average" class="pt-3" cols="12">
          <span>Average: {{ data.average }}</span>
        </v-col>
        <v-col v-if="data.median" class="pt-3" cols="12">
          <span>Median: {{ data.median }}</span>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";
export default {
  name: "PostSelect",
  methods: {
    loadDetail() {
      if (this.url != "" && this.port != "") {
        axios
          .get(`${this.url}/postCode/${this.selectedPost}`, { port: this.port })
          .then((response) => {
            console.log(response.data.payload);
            this.data = response.data.payload;
          });
      }
    },
  },
  props: {
    posts: {
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
      selectedPost: "",
      data: {},
    };
  },
};
</script>

<style lang="scss" scoped>
.margin-main {
  margin-left: 150px;
  margin-right: 150px;
  min-width: 1140px;
  min-height: 235px;
}
</style>