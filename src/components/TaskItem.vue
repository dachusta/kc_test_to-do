<script setup>
import VInput from "./UI/VInput.vue"
import VButton from "./UI/VButton.vue"
import VCheckbox from "./UI/VCheckbox.vue"

defineEmits(['remove', 'complete', 'edit'])

defineProps({
  task: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="task">
    <VCheckbox
      :checked="task.completed"
      @update:modelValue="$emit('complete', { id: task.id, completed: $event })"
    />

    <VInput
      type="text"
      :value="task.title"
      @input="$emit('edit', { id: task.id, title: $event.target.value })"
    />

    <div class="task__btns">
     <v-button
       @click="$emit('remove', task)"
     >
       Удалить
     </v-button>
    </div>
  </div>
</template>

<style scoped>
.task {
  padding: 15px;
  gap: 10px;
  border: 2px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
}

.task__btns {
  display: flex;
  gap: 10px;
}
</style>
