<template>
  <v-app>
    <app-bar :status="status"></app-bar>
    <v-main class="blue d-flex justify-center align-center">
      <div v-if="status !== 'Prompt Set' && status !== 'Testing'" class="d-flex justify-center">
        <v-btn @click="status = 'Ready'">Start The Test</v-btn>
      </div>
      <test-container
        :status="status"
        @prompt-set="status = 'Prompt Set'"
        @testing="status = 'Testing'"
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
    prompt: "",
    status: "Not Ready",
    difficulty: "medium",
    promptSet: false,
  }),

  computed: {
    breakpoint() {
      return this.$vuetify.breakpoint.name;
    },
  },

  methods: {
    stopTimer() {
      this.timesUp = true;
      return clearInterval(this.timerInterval);
    },
    reset() {
      this.input = "";
      this.timer = 60;
      this.timesUp = false;
    },
    wordsPerMinute() {
      let correct = document.querySelectorAll("span.gray"),
        incorrect = document.querySelectorAll("span.error");

      return Math.round((correct.length - incorrect.length) / 5);
    },
  },

  watch: {
    prompt: function() {
      if (this.prompt) {
        return this.promptSet = true;
      }
    }
  }
};
</script>

<style>
</style>
