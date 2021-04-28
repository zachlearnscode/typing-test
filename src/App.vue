<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <!-- <v-col> Timer: {{ timer | formatSecs }} </v-col> -->
          <v-col>
            <!-- <div v-if="timesUp">
              <span>{{ grossWordsPerMinute }}</span>
              <span>{{ numberOfMistakes }}</span>
              <span>{{ adjustedWordsPerMinute }}</span>
              <v-btn @click="reset">Reset</v-btn>
            </div> -->
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-container fluid>
              <v-row>
                <v-col
                  v-for="(word, w) in words"
                  :key="w"
                  cols="auto"
                  class="pa-0 d-flex"
                >
                  <span
                    v-for="(char, c) in word"
                    :key="c"
                    :id="currentIndex(w, c)"
                    style="white-space: pre"
                    >{{ char }}</span
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-col>
          <v-col>
            <textarea
              ref="textarea"
              :disabled="disabled"
              v-model="input"
            ></textarea>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    prompt:
      "In several countries history textbooks are tools to foster nationalism and patriotism, and give students the official line about national enemies. In many countries, history textbooks are sponsored by the national government and are written to put the national heritage in the most favourable light. For example, in Japan, mention of the Nanking Massacre has been removed from textbooks and the entire Second World War is given cursory treatment. Other countries have complained. It was standard policy in communist countries to present only a rigid Marxist historiography. In the United States, especially the southern part history about slavery and the American Civil War are controversial topics. McGraw-Hill Education for example, was criticised for describing Africans brought to American plantations as 'workers' instead of slaves in a textbook. Academic historians have often fought against the politicization of the textbooks, sometimes with success. In 21st-century Germany, the history curriculum is controlled by the 16 states, and is characterized not by superpatriotism but rather by an 'almost pacifistic and deliberately unpatriotic undertone' and reflects 'principles formulated by international organizations such as UNESCO or the Council of Europe, thus oriented towards human rights, democracy and peace.' The result is that 'German textbooks usually downplay national pride and ambitions and aim to develop an understanding of citizenship centered on democracy, progress, human rights, peace, tolerance and Europeanness.'",
    input: "",
    disabled: false,
    maxInputIndex: 0,
    // timer: 60,
    // timerInterval: null,
    // timesUp: false,
    // errors: [],
  }),

  computed: {
    words() {
      let result = this.prompt.split(" ").map((w) => (w += " "));

      result[result.length - 1] = result[result.length - 1].trim();

      return result;
    },
    currentInputIndex() {
      return this.input.length;
    },
  },

  methods: {
    currentIndex(w, c) {
      return this.words.slice(0, w).reduce((a, b) => a + b.length, 0) + c;
    },
    // startTimer() {
    //   this.timerInterval = setInterval(() => {
    //     this.timer--;
    //   }, 1000);
    // },
    // stopTimer() {
    //   return clearInterval(this.timerInterval);
    // },
    // reset() {
    //   this.input = "";
    //   this.timer = 60;
    //   this.timesUp = false;
    // },

    // addSpace(charIdx, lastIdxOfWord) {
    //   if (charIdx == lastIdxOfWord) {
    //     return " ";
    //   }

    //   return "";
    // },
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
    input: function () {
      let last = this.input.length - 1,
        secondToLast = this.input.length - 2;

      //Monitor for consecutive errors and disable textarea.
      if (
        this.input[last] !== this.prompt[last] &&
        this.input[secondToLast] !== this.prompt[secondToLast]
      ) {
        this.disabled = true;

        setTimeout(() => {
          this.input = this.input.slice(0, last);
          this.disabled = false;
        }, 250);
      }
    },

    //Refocus on textarea after it's been disabled -> reenabled
    disabled: function () {
      if (!this.disabled) {
        this.$nextTick(() => this.$refs.textarea.focus());
      }
    },

    currentInputIndex: function () {
      if (this.currentInputIndex > this.maxInputIndex) {
        return (this.maxInputIndex = this.currentInputIndex);
      }
    },

    timer: function () {
      if (this.timer == 0) {
        this.timesUp = true;
        return this.stopTimer();
      }
    },
  },
  mounted() {
    //
  },
  updated() {
    //Check for input accuracy and update styling
    let el = document.getElementById(this.currentInputIndex),
      spans = document.querySelectorAll("span");

    let forEach = Array.prototype.forEach;

    forEach.call(spans, (s, i) => {
      if (i >= this.currentInputIndex) {
        s.classList = "";
      } else {
        this.input[i] === this.prompt[i]
          ? (spans[i].classList = "gray")
          : (spans[i].classList = "error");
      }
    });

    if (!this.disabled) {
      el.classList = "blue";
    }
  },
};
</script>

<style>
* {
  font-family: "Courier New", Courier, monospace;
  font-size: 2rem;
}
.blue {
  background-color: blue;
  color: white;
  border-radius: 5px;
  padding: 1px;
}
.error {
  background-color: red;
  color: white;
  border-radius: 5px;
  padding: 1px;
}
.gray {
  color: gray;
}
</style>
