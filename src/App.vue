<template>
  <main class="container">
    <div class="theme-toggle">
      <button @click="toggleTheme" aria-label="Changer le thème">
        <img
          :src="theme === 'light' ? moonIcon : sunIcon"
          :alt="theme === 'light' ? 'Passer au mode sombre' : 'Passer au mode clair'"
          class="theme-icon"
        />
      </button>
    </div>

    <h1 class="main-title">Ma TodoList</h1>
    <TodoForm @add-task="addTask" />
    <TodoList :tasks="tasks" @update-task="updateTask" @delete-task="deleteTask" />
  </main>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

// Import des icônes
import moonIcon from './assets/img/moon.svg'
import sunIcon from './assets/img/sun.svg'

const theme = ref('light')

// Gestion du thème
function toggleTheme() {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
  document.documentElement.setAttribute('data-theme', theme.value)
}

onMounted(() => {
  document.documentElement.setAttribute('data-theme', theme.value)
})

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
    task.title = updatedTask.title // --- Pour la réédition 
    task.status = updatedTask.status // --- Pour le "choix" du status de la tache
  }
}

// Supprimer une tâche
function deleteTask(taskId) {
  tasks.value = tasks.value.filter(t => t.id !== taskId)
}

// Sauvegarde auto dans localStorage
watch(tasks, () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}, { deep: true })
</script>
