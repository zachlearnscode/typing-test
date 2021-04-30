<template>
  <span
    :class="{
      blue: isBlue,
      gray: isGray,
      error: isError,
      animate__headShake: isError,
    }"
    class="pa-1 animate__animated animate_faster"
    >{{ character }}</span
  >
</template>

<script>
export default {
  props: [
    "character",
    "characterIndex",
    "numCharsToWord",
    "textInput",
    "disabled",
  ],
  computed: {
    indexWithinPrompt() {
      return this.numCharsToWord + this.characterIndex;
    },
    textInputIndex() {
      return this.textInput.length;
    },
    isBlue() {
      return this.indexWithinPrompt == this.textInputIndex;
    },
    isGray() {
      return (
        this.indexWithinPrompt < this.textInputIndex &&
        this.character === this.textInput[this.indexWithinPrompt]
      );
    },
    isError() {
      if (!this.disabled) {
        return (
          this.indexWithinPrompt < this.textInputIndex &&
          this.character !== this.textInput[this.indexWithinPrompt]
        );
      } else {
        return (
          this.indexWithinPrompt <= this.textInputIndex &&
          this.character !== this.textInput[this.indexWithinPrompt]
        );
      }
    },
  },
};
</script>

<style scoped>
span {
  font-family: "Courier New", Courier, monospace;
  font-size: 2rem;
  white-space: pre;
}
.blue {
  background-color: blue;
  color: white;
  border-radius: 5px;
  padding: 1px;
  transition: all 0.1s ease;
}
.error {
  background-color: red;
  color: white;
  border-radius: 5px;
  padding: 1px;
}
.gray {
  color: lightgray;
}
</style>