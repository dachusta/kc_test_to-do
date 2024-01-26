<script setup>
import { ref, onMounted, computed, watch } from "vue"
import TaskList from "./components/TaskList.vue"
import VButton from "./components/UI/VButton.vue"
import VSelect from "./components/UI/VSelect.vue"
import VInput from "./components/UI/VInput.vue"

const tasks = ref([])
const newTask = ref('')
const isTasksLoading = ref(false)

function createTask() {
  tasks.value.push({
    id: Date.now(),
    completed: false,
    title: newTask.value,
  })
  newTask.value = ''
}

function completeTask(completedTask) {
  tasks.value.forEach(task => {
    if (task.id === completedTask.id) task.completed = completedTask.completed
  })
}

function editTask(editedTask) {
  tasks.value.forEach(task => {
    if (task.id === editedTask.id) task.title = editedTask.title
  })
}

function removeTask(task) {
  tasks.value = tasks.value.filter(p => p.id !== task.id)
}

async function fetchTasks() {
  try {
    isTasksLoading.value = true
    const response = await fetch('https://jsonplaceholder.typicode.com/todos?userId=1')
    tasks.value = await response.json().then(data => data.splice(0, 10))
  } catch (e) {
    alert('Ошибка')
  } finally {
    isTasksLoading.value = false
  }
}

const selectedFilter = ref('')
const filterOptions = ref([
  { value: '', name: 'Все' },
  { value: 'true', name: 'Выполненные' },
  { value: 'false', name: 'Невыполненные' },
])

const sortedTasks = computed(() => {
  return [...tasks.value].sort((task1, task2) => (task2.id - task1.id))
})

const filteredTasks = computed(() => {
  if (selectedFilter.value === '') {
    return sortedTasks.value
  } else {
    return sortedTasks.value.filter(task => {
      return task.completed.toString() === selectedFilter.value
    })
  }
})

onMounted(() => {
  if (localStorage.getItem('tasks')) {
    try {
      tasks.value = JSON.parse(localStorage.getItem('tasks'))
    } catch (e) {
      localStorage.removeItem('tasks')
    }
  } else {
    fetchTasks()
  }
})

watch(tasks, tasks => {
  localStorage.setItem('tasks', JSON.stringify(tasks))
}, { deep: true })

</script>

<template>
  <div class="app">
    <h1 class="app__title">Страница с задачами</h1>

    <v-input
      v-model="newTask"
      placeholder="Добавить задачу"
    />

    <div class="app__btns">
      <v-button
        class="btn__create"
        @click="createTask"
      >
        Создать задачу
      </v-button>

      <v-select
        class="btn__select"
        v-model="selectedFilter"
        :options="filterOptions"
      />
    </div>

    <task-list
      v-if="!isTasksLoading"
      :tasks="filteredTasks"
      @complete="completeTask"
      @edit="editTask"
      @remove="removeTask"
    />

    <div v-else>Идет загрузка...</div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app {
  max-width: 1440px;
  margin: auto;
  padding: 20px;
}

.app__title {
  margin-bottom: 30px;
}

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.btn__select {
  width: 160px;
}
</style>
