<template>
  <v-container
    fluid
    class="white"
    style="
      overflow: hidden;
      max-height: calc(100vh - 80px);
      box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.5);
    "
  >
    <v-row
      class="pa-5"
      style="position: relative; transition: all 250ms ease"
      :style="{ bottom: offset }"
    >
      <word
        v-for="(word, i) in words"
        :key="i"
        :word="word"
        :numCharsToWord="numCharsToWord(i)"
        :textInput="textInput"
        :disabled="disabled"
        class="animate__animated"
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
      return this.words.slice(0, idx).reduce((a, b) => a + b.length, 0);
    },
    appear(el) {
      let delay = el.dataset.index * 150;
      setTimeout(() => {
        el.classList.add("animate__fadeIn");
      }, delay);
    },
  },

  watch: {
    textInput: function () {
      if (document.querySelector(".cursor")) {
        return (this.offset =
          document.querySelector(".cursor").offsetTop + "px");
      }
    },
  },
};
</script>

<style>
</style>