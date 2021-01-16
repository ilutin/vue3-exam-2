<template>
  <div class="form-control">
    <label :for="id">{{ label }}</label>
    <select :id="id" @change="onChange">
      <option v-for="item in options" :value="item.value" :key="item.value">
        {{ item.text }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  emits: {
    change(value) {
      if (!value) return false;
      return true;
    },
  },
  props: {
    id: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      required: true,
    },
    label: String,
  },
  methods: {
    onChange(event) {
      this.$emit("change", event.target.value);
    },
  },
};
</script>

<style>
</style>
