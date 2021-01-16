<template>
  <div class="form-control">
    <label :for="id">{{ label }}</label>
    <textarea
      :id="id"
      :rows="rows"
      :value="modelValue"
      @input="onInput"
    ></textarea>
  </div>
</template>

<script>
export default {
  emits: ["update:modelValue"],
  props: {
    modelValue: String,
    id: {
      type: String,
      required: true,
    },
    label: String,
    rows: String,
  },
  methods: {
    onInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>
