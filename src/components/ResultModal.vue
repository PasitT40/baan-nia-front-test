<template>
  <div class="text-center">
    <v-dialog v-model="result.show" width="500">
      <v-card>
        <v-card-text>
          <v-row no-gutters>
            <v-col
              v-if="
                result.type == 'create' ||
                result.type == 'update' ||
                result.type == 'delete'
              "
              cols="12"
              class="mt-3 mb-3 d-flex justify-center"
            >
              <v-icon color="green" class="text-h1" x-large
                >mdi-check-circle</v-icon
              >
            </v-col>
            <v-col
              v-if="result.type == 'fail'"
              cols="12"
              class="mt-3 mb-3 d-flex justify-center"
            >
              <v-icon color="red" class="text-h1" x-large
                >mdi-close-circle</v-icon
              >
            </v-col>
            <v-col
              v-if="
                result.type == 'create' ||
                result.type == 'update' ||
                result.type == 'delete'
              "
              cols="12"
              class="mt-1 mb-1 d-flex justify-center"
            >
              <span>Success</span>
            </v-col>
            <v-col
              v-if="result.type == 'fail'"
              cols="12"
              class="mt-1 mb-1 d-flex justify-center"
            >
              <span>Fail</span>
            </v-col>
            <v-col
              v-if="result.type == 'fail'"
              cols="12"
              class="mt-1 mb-3 d-flex justify-center"
            >
              <span>let's try one more again</span>
            </v-col>
            <v-col
              v-if="result.type == 'update'"
              cols="12"
              class="mt-1 mb-3 d-flex justify-center"
            >
              <span>Update a Succesfull</span>
            </v-col>
            <v-col
              v-if="result.type == 'delete'"
              cols="12"
              class="mt-1 mb-3 d-flex justify-center"
            >
              <span>Delete a Succesfull</span>
            </v-col>
            <v-col
              v-if="result.type == 'create'"
              cols="12"
              class="mt-1 mb-3 d-flex justify-center"
            >
              <span>Create a Succesfull</span>
            </v-col>
            <v-col cols="12" class="mt-1 mb-1 d-flex justify-center">
              <v-btn @click="result.show = false" outlined>
                <span>Continue</span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "ResultModal",
  props: {
    result: {
      required: true,
    },
  },
};
</script>

<style>
</style>