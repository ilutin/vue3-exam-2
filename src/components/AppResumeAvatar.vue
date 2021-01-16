<template>
  <div class="avatar">
    <img :src="src" />
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      required: true,
    },
  },
};
</script>

<style>
</style>
