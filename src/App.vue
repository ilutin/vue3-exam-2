<template>
  <AppAlert :alert="alert" @close="alert = null" />

  <div class="container column">
    <form class="card card-w30">
      <AppSelect
        id="type"
        :options="selectOptions"
        label="Тип блока"
        @change="changeType"
      />

      <AppTextarea id="value" label="Значение" rows="3" v-model="value" />

      <app-button
        type="submit"
        classType="primary"
        @click="add"
        :disabled="isButtonDisabled"
      >
        Добавить
      </app-button>
    </form>

    <div class="card card-w70">
      <div v-if="this.title || this.avatar || this.description.length">
        <app-resume-title>{{ title }}</app-resume-title>

        <AppResumeAvatar :src="avatar" />

        <template v-for="block in this.description" :key="block.subtitle">
          <AppResumeDescription :subtitle="block.subtitle" :text="block.text" />
        </template>
      </div>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>

  <div class="container">
    <app-button
      classType="primary"
      :disabled="isSaveDisabled"
      @click="saveResume"
    >
      Сохранить Резюме
    </app-button>
    <AppComments />
  </div>
</template>

<script>
import AppSelect from "@/components/AppSelect.vue";
import AppTextarea from "@/components/AppTextarea.vue";
import AppButton from "@/components/AppButton.vue";
import AppResumeTitle from "@/components/AppResumeTitle.vue";
import AppResumeAvatar from "@/components/AppResumeAvatar.vue";
import AppResumeDescription from "@/components/AppResumeDescription.vue";
import AppComments from "@/components/AppComments.vue";
import AppAlert from "@/components/AppAlert.vue";

export default {
  data() {
    return {
      id: "",
      type: "title",
      value: "",
      title: "",
      avatar: "",
      description: [],
      selectOptions: [
        { value: "title", text: "Заголовок" },
        { value: "subtitle", text: "Подзаголовок" },
        { value: "avatar", text: "Аватар" },
        { value: "text", text: "Текст" },
      ],
      alert: null,
    };
  },
  mounted() {
    this.loadResume();
  },
  methods: {
    changeType(type) {
      this.type = type;
    },
    changeValue(value) {
      this.value = value;
    },
    add() {
      switch (this.type) {
        case "title": {
          this.title = this.value;
          break;
        }
        case "avatar": {
          this.avatar = this.value;
          break;
        }
        case "subtitle": {
          this.description.push({
            subtitle: this.value,
          });
          break;
        }
        case "text": {
          if (this.description.length > 0) {
            this.description[this.description.length - 1].text = this.value;
          }
          break;
        }
        default:
          break;
      }

      this.value = "";
    },
    async saveResume() {
      try {
        await fetch(
          "https://vue-with-http-21398-default-rtdb.europe-west1.firebasedatabase.app/resume.json",
          {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              title: this.title,
              avatar: this.avatar,
              description: this.description,
            }),
          }
        );

        this.alert = {
          type: "primary",
          title: "Успешно!",
          text: `Резюме успешно сохранено`,
        };
      } catch (e) {
        this.alert = {
          type: "danger",
          title: "Ошибка!",
          text: e.message,
        };
      }
    },
    async loadResume() {
      try {
        const response = await fetch(
          "https://vue-with-http-21398-default-rtdb.europe-west1.firebasedatabase.app/resume.json",
          {
            method: "GET",
          }
        );
        const data = await response.json();

        const [{ id, title, avatar, description }] = Object.keys(data).map(
          (item) => ({
            id: item,
            ...data[item],
          })
        );

        this.id = id;
        this.title = title;
        this.avatar = avatar;
        this.description = description;
      } catch (e) {
        console.warn("Список резюме пуст");
      }
    },
  },
  computed: {
    isButtonDisabled() {
      return this.value.length < 4;
    },
    isSaveDisabled() {
      return !(this.title && this.avatar && this.description.length);
    },
  },
  components: {
    AppSelect,
    AppTextarea,
    AppButton,
    AppResumeTitle,
    AppResumeAvatar,
    AppResumeDescription,
    AppComments,
    AppAlert,
  },
};
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
