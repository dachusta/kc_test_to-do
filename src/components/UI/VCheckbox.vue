<script setup>
defineEmits(['update:modelValue'])

defineProps({
  modelValue: {
    type: Boolean,
    default: false
  }
})
</script>

<template>
  <input
    class="checkbox"
    type="checkbox"
    :checked="modelValue"
    @change="$emit('update:modelValue', $event.target.checked)"
  >  
</template>

<style scoped>
.input {
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
</style>
