<script setup>
import { computed } from "vue"
import TaskItem from "./TaskItem.vue"

defineEmits(['remove', 'complete', 'edit'])

const props = defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

const completedTasks = computed(() => {
  return props.tasks.filter(task => task.completed)
})
const uncompletedTasks = computed(() => {
  return props.tasks.filter(task => !task.completed)
})
</script>

<template>
  <div v-if="tasks.length > 0">
    <h3
      v-if="uncompletedTasks.length"
      class="task-list__header"
    >
      Текущие задачи
    </h3>

    <transition-group name="task-list">
      <task-item
        v-for="task in uncompletedTasks"
        :task="task"
        :key="task.id"
        @complete="$emit('complete', $event)"
        @edit="$emit('edit', $event)"
        @remove="$emit('remove', $event)"
      />
    </transition-group>

    <h3
      v-if="completedTasks.length"
      class="task-list__header"
    >
      Выполненные задачи
    </h3>

    <transition-group name="task-list">
      <task-item
        v-for="task in completedTasks"
        :task="task"
        :key="task.id"
        @completed="$emit('completed', $event)"
        @edit="$emit('edit', $event)"
        @remove="$emit('remove', $event)"
      />
    </transition-group>
  </div>

  <h2 v-else style="color: red">
    Список задач пуст
  </h2>
</template>

<style scoped>
.task-list__header {
  margin-top: 30px;
}
.task-list-item {
  display: inline-block;
  margin-right: 10px;
}
.task-list-enter-active,
.task-list-leave-active {
  transition: all 0.2s ease;
}
.task-list-enter-from,
.task-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.task-list-move {
  transition: transform 0.2s ease;
}
</style>
