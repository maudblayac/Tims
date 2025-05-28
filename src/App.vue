<template>
  <main class="container">
    <h1>📝 Ma To-Do List Vue.js</h1>
    <TodoForm @add-task="addTask" />
    <TodoList
      :tasks="tasks"
      @update-task="updateTask"
      @delete-task="deleteTask"
    />
  </main>
</template>

<script setup>
import { ref, watch } from 'vue'
import TodoForm from '/components/TodoForm.vue'
import TodoList from '/components/TodoList.vue'

// 1. Liste des tâches
const tasks = ref(JSON.parse(localStorage.getItem('tasks') || '[]'))

// 2. Ajouter une tâche
function addTask(task) {
  tasks.value.push({
    id: Date.now(),
    title: task,
    status: 'todo',
  })
}

// 3. Modifier une tâche (par ex : changer son statut)
function updateTask(updatedTask) {
  const index = tasks.value.findIndex(t => t.id === updatedTask.id)
  if (index !== -1) {
    tasks.value[index] = { ...updatedTask }
  }
}

// 4. Supprimer une tâche
function deleteTask(taskId) {
  tasks.value = tasks.value.filter(t => t.id !== taskId)
}

// 5. Sauvegarde auto dans localStorage
watch(tasks, () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}, { deep: true })
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  padding: 2rem;
  font-family: sans-serif;
}
</style>
