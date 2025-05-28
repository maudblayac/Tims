<template>
  <section class="todo-form-section">
    <form @submit.prevent="submitTask" class="todo-form">
      <input
        v-model="newTask"
        type="text"
        placeholder="Ajouter une nouvelle tâche"
        required
        class="task-input"
      />
      <button type="submit" class="button button-add">Ajouter</button>
    </form>
  </section>
</template>

<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['add-task']) 
const newTask = ref('')

function submitTask() {
  if (newTask.value.trim() === '') return
  emit('add-task', newTask.value.trim())
  newTask.value = '' 
}
</script>
