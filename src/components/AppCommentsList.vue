<template>
  <ul class="list">
    <li class="list-item" v-for="comment in comments" :key="comment.id">
      <AppCommentsItem :email="comment.email" :body="comment.body" />
    </li>
  </ul>
</template>

<script>
import AppCommentsItem from "@/components/AppCommentsItem.vue";

export default {
  props: {
    comments: { type: Array, required: true },
  },
  components: {
    AppCommentsItem,
  },
};
</script>

<style>
</style>
