<script setup>
import {ref, onMounted } from "vue"

const tasks = ref([])

const loadTasks = async () => {
  const saved = localStorage.getItem('tasks')
  if (saved) {
    tasks.value = JSON.parse(saved)
  } else {
    const response = await fetch(`${import.meta.env.BASE_URL}tasks.json`)
    const data = await response.json()
    tasks.value = data
    localStorage.setItem('tasks', JSON.stringify(data))
  }
}
onMounted(() => {
  try {
    loadTasks()
  } catch (error) {
    console.error('Ошибка загрузки данных:', error)
  }
})


const toggleTask = (taskId) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.done = !task.done
    localStorage.setItem('tasks', JSON.stringify(tasks.value))
  }
}

</script>

<template>

   <li v-for="task in tasks" :key="task.id">
        <label > <input type="checkbox"  :checked="task.done"  @change="toggleTask(task.id)">  {{ task.title  }} </label> 
    </li>
     

</template>


