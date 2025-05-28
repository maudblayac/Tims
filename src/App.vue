<template>
  <main class="container">
    <h1>Ma TodoList</h1>
    <TodoForm 
      @add-task="addTask" 
    />
    <TodoList
      :tasks="tasks"
      @update-task="updateTask"
      @delete-task="deleteTask"
    />
  </main>
</template>

<script setup>
import { ref, watch } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

// Liste des tâches
const tasks = ref(JSON.parse(localStorage.getItem('tasks') || '[]'))

// Ajouter une tâche
function addTask(task) {
  tasks.value.push({
    id: Date.now(),
    title: task,
    status: 'todo',
  })
}

// Modifier une tâche (par ex : changer son statut (Revenir dessus plus tard))
function updateTask(updatedTask) {
  const task = tasks.value.find(t => t.id === updatedTask.id)
  if (task) {
    task.status = updatedTask.status
  }
}

// Supprimer une tâche
function deleteTask(taskId) {
  tasks.value = tasks.value.filter(t => t.id !== taskId)
}

// 5. Sauvegarde auto dans localStorage (plustard si ajout d'une BDD dans la bdd avec système de connexion via PDO voir API)
watch(tasks, () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}, { deep: true })
</script>

