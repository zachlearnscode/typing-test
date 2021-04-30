<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col> Timer: {{ timer | formatSecs }} </v-col>
          <v-col>
            <div>
              <!-- <span>WPM: {{ wordsPerMinute() }}</span> -->
              <v-btn @click="$refs.textarea.focus()"><div>Focus</div></v-btn>
            </div>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <prompt
              :prompt="prompt"
              :textInput="textInput"
              :textInputIndex="textInputIndex"
              :disabled="disabled"
            ></prompt>
          </v-col>
          <v-col>
            <text-input :value="textInput" :prompt="prompt" :disabled="disabled" @input="textInput = $event" ref="textInput"></text-input>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Prompt from './components/Prompt.vue';
import TextInput from './components/TextInput.vue';

export default {
  name: "App",

  components: {Prompt, TextInput},

  data: () => ({
    prompt:
      "In several countries history textbooks are tools to foster nationalism and patriotism, and give students the official line about national enemies. In many countries, history textbooks are sponsored by the national government and are written to put the national heritage in the most favourable light.",
    textInput: "",
    disabled: false,
    testStarted: false,
    timer: 60,
    timerInterval: null,
    timesUp: false,
  }),

  computed: {
    consecutiveErrors() {
      let last = this.textInput.length - 1,
        secondToLast = this.textInput.length - 2;

      return this.textInput[last] !== this.prompt[last] &&
      this.textInput[secondToLast] !== this.prompt[secondToLast]
    }
  },

  methods: {
    checkForConsecutiveErrors() {
      if (this.consecutiveErrors) {
        this.disabled = true;
        this.textInput = this.textInput
          .slice(0, this.textInput.length - 1);
        
        return this.reenable();
      }
    },
    reenable() {
      setTimeout(() => {
        this.disabled = false;
        this.$nextTick(() => this.$refs.textInput.$el.focus())
      }, 250)
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
    textInput: ["checkForConsecutiveErrors", "calculateOffset"],

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
