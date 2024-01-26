<script setup>
defineEmits(['update:modelValue'])

defineProps({
  modelValue: {
    type: String
  },
  options: {
    type: Array,
    default: () => []
  }
})
</script>

<template>
  <select
    class="select"
    :value="modelValue"
    @change="$emit('update:modelValue', $event.target.value)"
  >
    <option disabled value="">Выберите из списка</option>
    <option
      v-for="option in options"
      :key="option.value"
      :value="option.value"
    >
      {{ option.name }}
    </option>
  </select>
</template>

<style scoped>
.select {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
  cursor: pointer;
}
</style>
