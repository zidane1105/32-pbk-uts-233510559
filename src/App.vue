<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const input = ref('')
const filter = ref('all')

const addTask = () => {
  if (input.value.trim()) {
    tasks.value.push({
      id: Date.now(),
      name: input.value,
      completed: false
    })
    input.value = ''
  }
}

const removeTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

const filteredTasks = computed(() => {
  if (filter.value === 'all') return tasks.value
  if (filter.value === 'completed') return tasks.value.filter(task => task.completed)
})

const removeCompleted = () => {
  tasks.value = tasks.value.filter(task => !task.completed)
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-800 flex justify-center items-start p-10">
    <div class="w-full max-w-3xl bg-white rounded-lg shadow-md p-6 space-y-6">
      <h2 class="text-2xl font-bold text-center border-b pb-2">Daftar Tugas</h2>

      <!-- Input -->
      <div class="flex gap-2">
        <input
          v-model="input"
          @keyup.enter="addTask"
          placeholder="Masukkan tugas baru..."
          class="flex-1 px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <button
          @click="addTask"
          class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md font-medium"
        >Tambah</button>
      </div>

      <!-- Filter -->
      <div class="space-x-2 text-center">
        <button
          @click="filter = 'all'"
          :class="filter === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-200'"
          class="px-4 py-2 rounded-md font-medium"
        >Semua</button>
        <button
          @click="filter = 'completed'"
          :class="filter === 'completed' ? 'bg-blue-500 text-white' : 'bg-gray-200'"
          class="px-4 py-2 rounded-md font-medium"
        >Selesai</button>
        <button
          @click="removeCompleted"
          class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-md font-medium"
        >
          Hapus Tugas Selesai
        </button>
      </div>

      <ul class="space-y-3 h-70 overflow-y-auto">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          class="flex justify-between items-center bg-gray-100 p-3 rounded-md hover:bg-gray-200 transition"
        >
          <span :class="{ 'line-through text-gray-400': task.completed }">{{ task.name }}</span>
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="task.completed" class="accent-blue-500" />
            <button
              @click="removeTask(task.id)"
              class="text-red-500 hover:text-red-700 font-medium"
            >Hapus</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
</style>
