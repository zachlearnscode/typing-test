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
            <span
              v-for="(char, c) in prompt"
              :key="c"
              style="position:relative;"
              :style="{top: c >= currentWord.pre && c < currentWord.suc ? '2px' : '', color: c === currentCharacter ? 'red' : '', backgroundColor: char === prompt[c] ? '' : 'red'}"
            >{{char}}</span>
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
    spacesInPrompt() {
      let indexes = [];

      for (let i = 0; i < this.prompt.length; i++) {
        if (this.prompt[i] === " ") {
          indexes.push(i);
        }
      }

      return indexes;
    },
    numberOfSpacesInInput() {
      if (!this.input) {
        return 0;
      }

      return this.input.split("").filter((c) => c === " ").length;
    },
    currentWordOfInput() {
      let arr = this.input.split(' ');

      return arr[arr.length - 1];
    },
    precedingChars() {
      return this.prompt.slice(0, this.totalCurrentIdx + 1);
    },
    totalCurrentIdx() {
      if (this.idxOfSpacePrecedingCurrentWord) {
        if (this.currentWordOfInput) {
          return this.idxOfSpacePrecedingCurrentWord + this.currentWordOfInput.length;
        } else {
          return this.idxOfSpacePrecedingCurrentWord;
        }
      } else {
        if (this.currentWordOfInput) {
          return this.currentWordOfInput.length - 1
        } else {
          return -1;
        }
      }
    },
    currentCharacter() {
      //Split input by spaces -- the same way the prompt is split
      //to determine the current word -- locate the last word in the input
      //, and return the character of the current word at the position
      //equal to the length of the last word in the input.
      return this.totalCurrentIdx + 1;
      // return this.prompt[this.totalCurrentIdx + 1];
    },
    remainingPrompt() {
      return this.prompt.slice(this.totalCurrentIdx + 2);      
    },
    idxOfSpacePrecedingCurrentWord() {
      return this.spacesInPrompt[this.numberOfSpacesInInput - 1];
    },
    idxOfSpaceSucceedingCurrentWord() {
      return this.spacesInPrompt[this.numberOfSpacesInInput];
    },
    currentWord() {
      //The number of white spaces in the input should correspond
      //to the indexes of white spaces separating words in the prompt.
      if (this.numberOfSpacesInInput == 0) {
        return {pre: 0, suc: this.idxOfSpaceSucceedingCurrentWord + 1}
      } else {
        return {pre: this.idxOfSpacePrecedingCurrentWord + 1, suc: this.idxOfSpaceSucceedingCurrentWord}
        // return this.prompt.slice(
        //   this.idxOfSpacePrecedingCurrentWord + 1,
        //   this.idxOfSpaceSucceedingCurrentWord
        // );
      }
    },
    // promptUpToCurrentWord() {
    //   let idx = this.numberOfWhiteSpacesInInput
    //     ? this.indexesOfWhiteSpaceInPrompt[this.numberOfWhiteSpacesInInput - 1]
    //     : 0;
    //   return this.prompt.slice(0, idx);
    // },
    // promptAfterCurrentWord() {
    //   return this.prompt.slice(
    //     this.indexesOfWhiteSpaceInPrompt[this.numberOfWhiteSpacesInInput]
    //   );
    // },
    // currentWordUpToCurrentChar() {
    //   let currentWord = this.currentWord;

    //   let inputArr = this.input.split(" ");

    //   let lastWord = inputArr[inputArr.length - 1];

    //   return currentWord.slice(0, lastWord.length);
    // },
    // currentWordAfterCurrentChar() {
    //   let currentWord = this.currentWord;
    //   let inputArr = this.input.split(" "),
    //     lastWord = inputArr[inputArr.length - 1];

    //   return currentWord.slice(lastWord.length + 1);
    // },
    // // currentChar() {
    // //   let currentWord = this.currentWord;
    // //   let inputArr = this.input.split(" "),
    // //     lastWord = inputArr[inputArr.length - 1];

    // //   return currentWord[lastWord.length];
    // // },
    // charsUpToCurrentChar() {
    //   return this.promptUpToCurrentWord.concat(this.currentWordUpToCurrentChar);
    // },

    // grossWordsPerMinute() {
    //   return this.input.length / 5 / 1;
    // },
    // adjustedWordsPerMinute() {
    //   return (this.grossWordsPerMinute - this.numberOfMistakes) / 1;
    // },
    // numberOfMistakes() {
    //   let slicePrompt = this.prompt.slice(0, this.input.length);
    //   //let count = 0;

    //   return this.input.split("").filter((c, i) => c !== slicePrompt[i]).length;
    // },
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
