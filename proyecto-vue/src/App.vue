<template>
  <div class="container">
    <h1>Gestor de Tareas</h1>

    <!-- Input para agregar tareas -->
    <input
      v-model="newTask"
      placeholder="Escribe el nombre de la tarea"
      @keyup.enter="addTask"
    />
    <button @click="addTask">Agregar</button>

    <hr />

    <!-- Mostrar mensaje si no hay tareas -->
    <div v-if="tasks.length === 0">
      <p>No hay tareas registradas.</p>
    </div>

    <!-- Mostrar las tres secciones -->
    <div v-else class="sections">
      <div class="section">
        <h3>To do</h3>
        <ul>
          <li v-for="(task, index) in toDoTasks" :key="index">
            {{ task.name }}
            <button @click="moveTask(task, 'Doing')">→</button>
          </li>
        </ul>
      </div>

      <div class="section">
        <h3>Doing</h3>
        <ul>
          <li v-for="(task, index) in doingTasks" :key="index">
            {{ task.name }}
            <button @click="moveTask(task, 'To do')">←</button>
            <button @click="moveTask(task, 'Done')">→</button>
          </li>
        </ul>
      </div>

      <div class="section">
        <h3>Done</h3>
        <ul>
          <li v-for="(task, index) in doneTasks" :key="index">
            {{ task.name }}
            <button @click="moveTask(task, 'Doing')">←</button>
            <!-- En Done no se elimina -->
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const tasks = ref([]) // { name: string, status: 'To do' | 'Doing' | 'Done' }

const addTask = () => {
  const name = newTask.value.trim()
  if (name !== '') {
    tasks.value.push({ name, status: 'To do' })
    newTask.value = ''
  }
}

const moveTask = (task, newStatus) => {
  task.status = newStatus
}

const toDoTasks = computed(() => tasks.value.filter(t => t.status === 'To do'))
const doingTasks = computed(() => tasks.value.filter(t => t.status === 'Doing'))
const doneTasks = computed(() => tasks.value.filter(t => t.status === 'Done'))
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  font-family: sans-serif;
  text-align: center;
}

input {
  padding: 8px;
  width: 60%;
}

button {
  margin-left: 5px;
  padding: 6px 10px;
}

.sections {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.section {
  width: 30%;
  background: #f8f8f8;
  padding: 10px;
  border-radius: 6px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: white;
  margin-bottom: 8px;
  padding: 6px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
