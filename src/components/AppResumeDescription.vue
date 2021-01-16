<template>
  <h2>{{ subtitle }}</h2>
  <p>{{ text }}</p>
</template>

<script>
export default {
  props: {
    subtitle: String,
    text: String,
  },
};
</script>

<style>
</style>
