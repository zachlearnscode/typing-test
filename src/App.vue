<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col> Timer: {{ timer | formatSecs }} </v-col>
          <v-col>
            <div v-if="timesUp">
              <span>{{ grossWordsPerMinute }}</span>
              <span>{{ numberOfMistakes }}</span>
              <span>{{ adjustedWordsPerMinute }}</span>
              <v-btn @click="reset">Reset</v-btn>
            </div>
          </v-col>
        </v-row>
        <v-row>
          <v-col class="pa-3" style="border-right: 1px solid black">
            <span id="promptUpToCurrentWord">{{promptUpToCurrentWord}}</span>
            <span id="currentWordUpToCurrentChar" style="position:relative;top:10px;">
              {{currentWordUpToCurrentChar}}
              <span id="currentChar" style="color:green;">{{currentChar}}</span>
            </span>
            <span id="currentWordAfterCurrentChar" style="position:relative;top:10px;">{{currentWordAfterCurrentChar}}</span>
            <span id="promptAfterCurrentWord">{{promptAfterCurrentWord}}</span>
          </v-col>
          <v-col>
            <v-textarea
              full-width
              outlined
              height="100vh"
              v-model="input"
              :disabled="timesUp"
            ></v-textarea>
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
    timer: 60,
    timerInterval: null,
    timesUp: false,
  }),

  computed: {
    indexesOfWhiteSpaceInPrompt() {
      let indexes = [];

      for (let i = 0; i < this.prompt.length; i++) {
        if (this.prompt[i] === " ") {
          indexes.push(i);
        }
      }

      return indexes;
    },
    numberOfWhiteSpacesInInput() {
      if (this.input) {
        return this.input.split("").filter((c) => c === " ").length;
      } else {
        return 0;
      }
    },
    currentWord() {
      //The number of white spaces in the input should correspond
      //to the indexes of white space separating words in the prompt.
      let atLeastZero =
        this.numberOfWhiteSpacesInInput - 1 > -1
          ? this.indexesOfWhiteSpaceInPrompt[
              this.numberOfWhiteSpacesInInput - 1
            ]
          : 0;
      return this.prompt
        .slice(
          atLeastZero,
          this.indexesOfWhiteSpaceInPrompt[this.numberOfWhiteSpacesInInput]
        )
        .trim();
    },
    promptUpToCurrentWord() {
      return this.prompt.slice(0, this.indexesOfWhiteSpaceInPrompt[this.numberOfWhiteSpacesInInput - 1])
    },
    promptAfterCurrentWord() {
      return this.prompt.slice(this.indexesOfWhiteSpaceInPrompt[this.numberOfWhiteSpacesInInput])
    },
    currentWordUpToCurrentChar() {
      let currentWord = this.currentWord;

      let inputArr = this.input.split(' ');

      let lastWord = inputArr[inputArr.length - 1];

      return currentWord.slice(0, lastWord.length)
    },
    currentWordAfterCurrentChar() {
      let currentWord = this.currentWord;
      let inputArr = this.input.split(' '),
          lastWord = inputArr[inputArr.length - 1];

      return currentWord.slice(lastWord.length + 1)
    },
    currentChar() {
      let currentWord = this.currentWord;
      let inputArr = this.input.split(' '),
          lastWord = inputArr[inputArr.length - 1];

      return currentWord[lastWord.length];
    },




    grossWordsPerMinute() {
      return this.input.length / 5 / 1;
    },
    adjustedWordsPerMinute() {
      return (this.grossWordsPerMinute - this.numberOfMistakes) / 1;
    },
    numberOfMistakes() {
      let slicePrompt = this.prompt.slice(0, this.input.length);
      //let count = 0;

      return this.input.split("").filter((c, i) => c !== slicePrompt[i]).length;
    },
  },

  methods: {
    startTimer() {
      this.timerInterval = setInterval(() => {
        this.timer--;
      }, 1000);
    },
    stopTimer() {
      return clearInterval(this.timerInterval);
    },
    reset() {
      this.input = "";
      this.timer = 60;
      this.timesUp = false;
    },

    addSpace(charIdx, lastIdxOfWord) {
      if (charIdx == lastIdxOfWord) {
        return " ";
      }

      return "";
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
      //this.numberOfWhiteSpacesInInput()
    },
    timer: function () {
      if (this.timer == 0) {
        this.timesUp = true;
        return this.stopTimer();
      }
    },
  },
};
</script>
