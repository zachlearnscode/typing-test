<template>
  <v-col cols="auto" class="d-flex pa-0">
    <character
      v-for="(character, i) in word"
      :key="i"
      :character="character"
      :characterIndex="i"
      :numCharsToWord="numCharsToWord"
      :textInput="textInput"
      :textInputIndex="textInputIndex"
      :disabled="disabled"
    ></character>
  </v-col>
</template>

<script>
import Character from "./Character.vue";

export default {
  props: ["word", "numCharsToWord", "textInput", "disabled"],

  components: {
    Character,
  },
};
</script>

<style>
</style>