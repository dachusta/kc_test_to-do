<script setup>
defineProps({
  modelValue: [String, Number]
})
</script>

<template>
  <input
    class="input"
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  >
</template>

<style scoped>
.input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
}
</style>
