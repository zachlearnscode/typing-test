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
            <v-container style="height: 70vh; overflow: hidden">
              <v-row
                id="promptContainer"
                style="position: relative; transition: all 100ms ease"
              >
                <v-col
                  v-for="(word, w) in words"
                  :key="w"
                  cols="auto"
                  class="pa-0 d-flex"
                >
                  <span
                    v-for="(char, c) in word"
                    :key="c"
                    :id="overallIndex(w, c)"
                    class="char animate__animated animate__faster"
                    style="white-space: pre"
                    >{{ char }}</span
                  >
                </v-col>
              </v-row>
            </v-container>
          </v-col>
          <v-col style="position: absolute; z-index: -1">
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
    testStarted: false,
    timer: 60,
    timerInterval: null,
    timesUp: false,
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
    overallIndex(w, c) {
      return this.words.slice(0, w).reduce((a, b) => a + b.length, 0) + c;
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
    consecutiveErrors() {
      let disable = false;

      let current = this.input.length - 1,
        previous = this.input.length - 2;

      if (
        this.input[current] !== this.prompt[current] &&
        this.input[previous] !== this.prompt[previous]
      ) {
        disable = true;
      }

      return disable;
    },
    updateClassList(classList, classToAdd, classToRemove) {
      let result = [];

      classList.remove("blue");
      classList.forEach((c) => result.push(c));

      if (!result.includes(classToAdd)) {
        result.push(classToAdd);
      }

      if (result.includes(classToRemove)) {
        let idx = result.indexOf(classToRemove);

        result = result.slice(0, idx).concat(result.slice(idx + 1));
      }

      result = result.toString().replaceAll(",", " ");

      return result;
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
    input: function () {
      if (this.consecutiveErrors()) {
        this.disabled = true;
        this.input = this.input.slice(0, this.input.length - 1);
      }

      //Update prompt styling
      let forEach = Array.prototype.forEach;
      let spans = document.querySelectorAll(".char");      

      forEach.call(spans, (s, i) => {
        let condition = !this.disabled
          ? i < this.currentInputIndex
          : i <= this.currentInputIndex;

        if (condition) {
          this.input[i] === this.prompt[i]
            ? (spans[i].classList = this.updateClassList(
                spans[i].classList,
                "gray",
                "error"
              ))
            : (spans[i].classList = this.updateClassList(
                spans[i].classList,
                "error",
                "gray"
              ));
        }
      });

      let el = document
        .getElementById(this.currentInputIndex);

      if (!el.classList.contains("error")) {
        el.classList.add("blue");
      } else {
        el.classList.add("animate__headShake")
      }

      let offset = el.offsetTop;

      document.getElementById("promptContainer").style.bottom =
        String(offset) + "px";
    },

    //Refocus on textarea after it's been disabled -> reenabled
    disabled: function () {
      if (this.disabled) {
        setTimeout(() => this.disabled = false, 250);
      } else {
        setTimeout(() => this.$refs.textarea.focus(), 250);
      }
    },

    // currentInputIndex: function () {
    //   if (this.currentInputIndex == 1 && !this.testStarted) {
    //     return this.startTimer();
    //   }
    // },

    timer: function () {
      if (this.timer == 0) {
        return this.stopTimer();
      }
    },
  },
  mounted() {
    this.$refs.textarea.focus();
    let el = document.getElementById(this.currentInputIndex);

    el.classList.add("blue");
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
