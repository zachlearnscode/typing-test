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
};
</script>

<style>
</style>