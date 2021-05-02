<template>
  <v-app>
    <app-bar :status="status"></app-bar>
    <v-main class="blue d-flex justify-center align-center">
      <div v-if="!difficulty" class="d-flex justify-center">
        <v-btn @click="$nextTick(() => difficulty = 'medium')">Start The Test</v-btn>
      </div>
      <test-container
        v-else
        :difficulty="difficulty"
        @test-started="testStarted = true"
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
    difficulty: "",
    testStarted: false,
  }),

  computed: {
    consecutiveErrors() {
      let last = this.textInput.length - 1,
        secondToLast = this.textInput.length - 2;

      return (
        this.textInput[last] !== this.prompt[last] &&
        this.textInput[secondToLast] !== this.prompt[secondToLast]
      );
    },
    breakpoint() {
      return this.$vuetify.breakpoint.name;
    },
    status() {
      if (this.difficulty && !this.testStarted) {
        return "Ready";
      } else if (this.difficulty && this.testStarted) {
        return "Test Started";
      } else {
        return "Not Ready";
      }
    }
  },

  methods: {
    checkForConsecutiveErrors() {
      if (this.consecutiveErrors) {
        this.disabled = true;
        this.textInput = this.textInput.slice(0, this.textInput.length - 1);

        return this.reenable();
      }
    },
    reenable() {
      setTimeout(() => {
        this.disabled = false;
        this.$nextTick(() => this.$refs.textInput.$el.focus());
      }, 250);
    },
    startTimer() {
      this.timerInterval = setInterval(() => {
        this.timer--;
      }, 1000);
    },
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

  filters: {
    formatSecs(num) {
      if (num == 60) {
        return "1:00";
      } else {
        return ":" + String(num);
      }
    },
  },

  watch: {
    textInput: "checkForConsecutiveErrors",

    timer: function () {
      if (this.timer == 0) {
        return this.stopTimer();
      }
    },
  },
};
</script>

<style>
</style>
