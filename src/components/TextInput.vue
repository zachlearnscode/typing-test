<template>
  <textarea
    :value="value"
    @input.capture="$emit('input', $event.target.value)"
    :disabled="disabled"
  ></textarea>
</template>

<script>
export default {
  props: ["value", "disabled"],

  watch: {
    value: function() {
      if (this.value.length === 1) {
        return this.$emit('testing');
      }
    }
  }
};
</script>

<style>
</style>