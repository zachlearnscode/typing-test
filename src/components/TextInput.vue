<template>
  <textarea
    :value="value"
    @input="$emit('input', $event.target.value)"
    :disabled="disabled"
  ></textarea>
</template>

<script>
export default {
  props: ["value", "disabled"],

  watch: {
    value: function() {
      if (this.value.length === 1) {
        return this.$emit('test-started');
      }
    }
  }
};
</script>

<style>
</style>