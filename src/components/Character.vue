<template>
  <span
    :class="{
      cursor: isCurrent,
      animate__flash: isCurrent,
      gray: isGray,
      error: isError,
      animate__headShake: isError,
    }"
    class="animate__animated"
    :style="{fontSize: fontSize}"
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
    isCurrent() {
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
    fontSize() {
      let result;

      switch (this.$vuetify.breakpoint.name) {
        case "xs":
          result = '1.3rem';
          break;
        case "sm":
          result = '2rem';
          break;
        case "md":
          result = '2rem';
          break;
        case "lg":
          result = '2rem';
          break;
        case "xl":
          result = '2rem';
          break;
      }

      return result;
    }
  },
};
</script>

<style scoped>
span {
  font-family: "Courier New", Courier, monospace;
  white-space: pre;
  border-bottom: 2px solid transparent;
}
.cursor {
  border-bottom: 2px solid #2196F3;
  
}
.animate__animated.animate__flash {
  animation-iteration-count: infinite;
}
.error {
  background-color: #F44336;
  color: white;
  border-radius: 5px;
}
.gray {
  color: lightgray;
}
</style>