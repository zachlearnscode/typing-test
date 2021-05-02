<template>
  <v-app-bar app dark flat color="blue">
    <v-app-bar-title class="d-flex align-center"
      ><v-icon large class="mr-2">mdi-alpha-t-box</v-icon>Type Test</v-app-bar-title
    >
  </v-app-bar>
</template>

<script>
export default {
  props: ["breakpoint"],

  computed: {
    prominent() {
      let result;

      switch (this.breakpoint) {
        case "xs":
          result = false;
          break;
        case "sm":
          result = false;
          break;
        case "md":
          result = true;
          break;
        case "lg":
          result = true;
          break;
        case "xl":
          result = true;
          break;
      }

      return result;
    },
  },
};
</script>

<style>
</style>