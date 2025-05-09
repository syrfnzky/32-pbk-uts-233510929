<script setup>
import { ref } from 'vue'

const tasks = ref([])
const input = ref('')

const addTask = () => {
  if (input.value.trim() === '') return
  tasks.value.push({
    id: tasks.value.length + 1,
    text: input.value,
    completed: false
  })
  input.value = ''
}
</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" />
    <button @click="addTask">➕ Add Task</button>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
