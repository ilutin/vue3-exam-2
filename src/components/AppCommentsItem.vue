<template>
  <div>
    <p>
      <strong>{{ email }}</strong>
    </p>
    <small>
      {{ body }}
    </small>
  </div>
</template>

<script>
export default {
  props: {
    email: String,
    body: String,
  },
};
</script>

<style>
</style>
