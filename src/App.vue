<template>
  <v-app>
    <app-bar :status="status" @time-expired="status = 'Time Expired'"></app-bar>
    <v-main class="blue d-flex justify-center align-center">
      <div v-if="status === 'Not Ready'" class="d-flex justify-center">
        <v-btn @click="status = 'Ready'">Start The Test</v-btn>
      </div>
      <test-container
        :status="status"
        @prompt-set="status = 'Prompt Set'"
        @testing="status = 'Testing'"
        @dialog-ok="status = 'Not Ready'"
      ></test-container>
    </v-main>
  </v-app>
</template>

<script>
import AppBar from "./components/AppBar.vue";
import TestContainer from "./components/TestContainer.vue";

export default {
  name: "App",

  components: { AppBar, TestContainer },

  data: () => ({
    status: "Not Ready",
  }),

  methods: {
    reset() {
      this.status.notReady = true;
      this.status.ready = false;
      this.status.promptSet = false;
      this.status.testing = false;
      this.status.timeExpired = false;
    },
  },
};
</script>

<style>
</style>
