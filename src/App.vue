<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const input = ref('')
const filter = ref('all')

const addTask = () => {
  if (input.value.trim() === '') return
  tasks.value.push({
    id: tasks.value.length + 1,
    text: input.value,
    completed: false
  })
  input.value = ''
}

const removeTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

const countCompletedTasks = computed(() => {
  return tasks.value.filter(task => task.completed).length
})

const countTask = computed(() => {
  return tasks.value.length
})

const filteredTasks = computed(() => {
  if (filter.value === 'all') {
    return tasks.value
  } else if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed)
  }
  return []
})

</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" />
    <button @click="addTask">➕ Add Task</button>
    <div>
      <button @click="filter = 'all'">All</button>
      <button @click="filter = 'completed'">Completed</button>
      <button @click="filter = 'active'">Active</button>
    </div>

    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.text }}
        <button @click="removeTask(task.id)">❌</button>
      </li>
    </ul>

    <p>Completed Tasks: {{ countCompletedTasks }}</p>
    <p>Total Tasks: {{ countTask }}</p>
  </div>
</template>

<style scoped></style>
