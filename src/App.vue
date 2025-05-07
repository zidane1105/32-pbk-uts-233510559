<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const input = ref('')
const filter = ref('all')

const addTask = () => {
  if (input.value) {

    tasks.value.push({
      id: tasks.value.length + 1,
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
  if (filter.value === 'all') {
    return tasks.value
  } else if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  }
})

const removeCompleted = () => {
  tasks.value = tasks.value.filter(task => !task.completed)
}

</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>
    <div>
      <button @click="filter = 'all'">All</button>
      <button @click="filter = 'completed'">Completed</button>
    </div>
    <button @click="removeCompleted">Remove Completed</button>
    <ul>
      <li v-for="task in filteredTasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.name }}
        <button @click="removeTask(task.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
