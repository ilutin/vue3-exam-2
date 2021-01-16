<template>
  <button
    :type="type"
    :class="['btn', classType]"
    @click="onClick"
    :disabled="disabled"
  >
    <slot />
  </button>
</template>

<script>
export default {
  emits: ["click"],
  props: {
    type: {
      type: String,
      required: false,
    },
    classType: String,
    disabled: Boolean,
  },
  methods: {
    onClick(event) {
      if (this.type === "submit") event.preventDefault();
      this.$emit("click");
    },
  },
};
</script>
