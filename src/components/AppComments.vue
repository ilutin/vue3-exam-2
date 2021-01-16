<template>
  <p>
    <app-button classType="primary" @click="loadComments">
      Загрузить комментарии
    </app-button>
  </p>
  <div class="center" v-if="loading">
    <AppLoader />
  </div>
  <div class="card" v-else>
    <h2>Комментарии</h2>
    <AppCommentsList :comments="comments" />
  </div>
</template>

<script>
import AppButton from "@/components/AppButton.vue";
import AppCommentsList from "@/components/AppCommentsList.vue";
import AppLoader from "@/components/AppLoader.vue";

export default {
  data() {
    return {
      loading: false,
      comments: [],
    };
  },
  methods: {
    async loadComments() {
      this.loading = true;

      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/comments?_limit=42",
          {
            type: "GET",
          }
        );

        this.comments = await response.json();
      } catch (e) {
        console.error("Ошибка загрузки комментариев");
      }

      this.loading = false;
    },
  },
  components: {
    AppButton,
    AppCommentsList,
    AppLoader,
  },
};
</script>

<style>
</style>
