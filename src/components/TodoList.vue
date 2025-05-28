<template>
  <section class="todo-list">
    <article
      v-for="task in tasks"
      :key="task.id"
      :class="['task-item', task.status]"
    >
      <div class="task-content">
        <input
          v-if="editedTaskId === task.id"
          v-model="editedTitle"
          class="task-edit-input"
          @keyup.enter="validateEdit(task)"
          @keyup.esc="cancelEdit()"
          autofocus
        />
        <span v-else class="task-title">{{ task.title }}</span>
      </div>

      <div class="task-actions" :class="{ editing: editedTaskId === task.id }">
        <template v-if="editedTaskId === task.id">
          <button @click="cancelEdit" class="icon-btn" aria-label="Annuler">
            <img :src="crossIcon" alt="Annuler" />
          </button>
          <button @click="validateEdit(task)" class="icon-btn" aria-label="Valider">
            <img :src="checkIcon" alt="Valider" />
          </button>
        </template>
        <template v-else>
          <button @click="startEdit(task)" class="icon-btn" aria-label="Modifier">
            <img :src="editIcon" alt="Modifier" />
          </button>
          <button @click="deleteTask(task.id)" class="icon-btn" aria-label="Supprimer">
            <img :src="trashIcon" alt="Supprimer" />
          </button>
          <button
            @click="cycleStatus(task)"
            class="btn-status"
            :title="'Statut: ' + statusLabels[task.status]"
          >
            <img
              :src="statusIcons[task.status]"
              :alt="statusLabels[task.status]"
              class="status-icon"
            />
            <span class="status-text">{{ statusLabels[task.status] }}</span>
          </button>
        </template>
      </div>
    </article>
  </section>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue'

// Import des icônes
import todoIcon from '../assets/img/todo.svg'
import inProgressIcon from '../assets/img/in-progress.svg'
import doneIcon from '../assets/img/done.svg'
import crossIcon from '../assets/img/cross.svg'
import checkIcon from '../assets/img/check.svg'
import editIcon from '../assets/img/edit.svg'
import trashIcon from '../assets/img/trash.svg'

const props = defineProps({
  tasks: { type: Array, required: true }
})

const emit = defineEmits(['update-task', 'delete-task'])

const editedTaskId = ref(null)
const editedTitle = ref('')

// Statuts
const statusOrder = ['todo', 'in-progress', 'done']
const statusLabels = {
  'todo': 'À faire',
  'in-progress': 'En cours',
  'done': 'Terminé'
}
const statusIcons = {
  'todo': todoIcon,
  'in-progress': inProgressIcon,
  'done': doneIcon,
}

function startEdit(task) {
  editedTaskId.value = task.id
  editedTitle.value = task.title
}

function cancelEdit() {
  editedTaskId.value = null
  editedTitle.value = ''
}

function validateEdit(task) {
  if (editedTitle.value.trim() !== '') {
    emit('update-task', { ...task, title: editedTitle.value.trim() })
  }
  cancelEdit()
}

function cycleStatus(task) {
  const currentIndex = statusOrder.indexOf(task.status)
  const nextIndex = (currentIndex + 1) % statusOrder.length
  emit('update-task', { ...task, status: statusOrder[nextIndex] })
}

function deleteTask(taskId) {
  emit('delete-task', taskId)
}
</script>
