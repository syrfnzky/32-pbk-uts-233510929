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

const filterBtn ='px-4 py-1 border border-blue-300 rounded-full text-blue-600 hover:bg-blue-100 transition'
const activeFilter ='px-4 py-1 bg-blue-600 text-white rounded-full font-semibold shadow-sm'

</script>

<template>
  <div class="min-h-screen bg-blue-50 flex items-center justify-center p-6">
    <div class="bg-white w-full max-w-2xl rounded-2xl shadow-lg p-8 space-y-6 border border-blue-200">
      <h1 class="text-3xl font-bold text-blue-800 text-center">📋 Blue Task Manager</h1>

      <div class="flex gap-3">
        <input
          type="text"
          v-model="input"
          @keyup.enter="addTask"
          placeholder="Enter a task..."
          class="flex-1 px-4 py-2 border border-blue-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button
          @click="addTask"
          class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg font-semibold"
        >
          ➕ Add
        </button>
      </div>

      <div class="flex justify-center gap-2">
        <button
          @click="filter = 'all'"
          :class="filter === 'all' ? activeFilter : filterBtn"
        >
          All
        </button>
        <button
          @click="filter = 'completed'"
          :class="filter === 'completed' ? activeFilter : filterBtn"
        >
          Completed
        </button>
        <button
          @click="filter = 'active'"
          :class="filter === 'active' ? activeFilter : filterBtn"
        >
          Active
        </button>
      </div>

      <ul class="space-y-2 h-50 overflow-y-auto">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          class="flex items-center justify-between bg-blue-100 border border-blue-200 px-4 py-2 rounded-lg"
        >
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="task.completed" class="accent-blue-600 w-5 h-5" />
            <span :class="task.completed ? 'line-through text-blue-400' : 'text-blue-800'">
              {{ task.text }}
            </span>
          </div>
          <button
            @click="removeTask(task.id)"
            class="text-blue-700 hover:text-red-500 font-semibold text-lg"
          >
            ❌
          </button>
        </li>
      </ul>

      <!-- Stats -->
      <div class="text-sm text-center text-blue-700 font-medium pt-4">
        <p>✅ Completed Tasks: {{ countCompletedTasks }}</p>
        <p>📦 Total Tasks: {{ countTask }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
