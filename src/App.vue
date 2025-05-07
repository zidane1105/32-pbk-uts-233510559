<script setup>
import { ref } from 'vue'

const tasks = ref([])
const input = ref('')

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

</script>

<template>
  <div>
    <input type="text" v-model="input" @keyup.enter="addTask" />
    <button @click="addTask">Add Task</button>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        {{ task.name }}
        <button @click="removeTask(task.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
