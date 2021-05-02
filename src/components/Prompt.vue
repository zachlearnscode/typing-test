<template>
  <v-container class="white rounded-lg" style="overflow: hidden">
    <v-row
      class="pa-1"
      style="position: relative; transition: all 250ms ease;"
      :style="{ bottom: offset }"
    >
      <word
        v-for="(word, i) in words"
        :key="i"
        :word="word"
        :numCharsToWord="numCharsToWord(i)"
        :textInput="textInput"
        :disabled="disabled"
      >
      </word>
    </v-row>
  </v-container>
</template>

<script>
import Word from "./Word.vue";

export default {
  props: ["prompt", "textInput", "disabled"],

  components: {
    Word,
  },

  data() {
    return {
      offset: "",
    };
  },

  computed: {
    words() {
      let words = this.prompt.split(" ");

      words = words.map((w, i) => {
        if (i < words.length - 1) {
          w += " ";
        }
        return w;
      });

      return words;
    },
  },

  methods: {
    numCharsToWord(idx) {
      return this.words
      .slice(0, idx)
      .reduce((a, b) => a + b.length, 0);
    },
  },

  watch: {
    textInput: function () {
      return this.offset = document.querySelector(".cursor").offsetTop + "px";
    },
  },
};
</script>

<style>
</style>