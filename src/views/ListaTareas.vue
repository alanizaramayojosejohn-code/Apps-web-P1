<script setup>
import { ref } from 'vue'

const nuevaTarea = ref('')
const tareas = ref([])

function agregarTarea() {
  const texto = nuevaTarea.value.trim()
  if (!texto) return
  tareas.value.push(texto)
  nuevaTarea.value = ''
}

function eliminarTarea(index) {
  tareas.value.splice(index, 1)
}
</script>

<template>
  <div class="max-w-xl mx-auto bg-white rounded-lg shadow p-6">
    <h2 class="text-xl font-bold text-slate-800 mb-4">Lista de Tareas</h2>

    <div class="flex gap-2 mb-5">
      <input
        v-model="nuevaTarea"
        @keyup.enter="agregarTarea"
        type="text"
        placeholder="Escribe una nueva tarea..."
        class="flex-1 border border-slate-300 rounded-md px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
      <button
        @click="agregarTarea"
        class="px-4 py-2 rounded-md bg-blue-600 text-white text-sm font-medium hover:bg-blue-700 transition-colors whitespace-nowrap"
      >
        + Agregar Tarea
      </button>
    </div>

    <p v-if="tareas.length === 0" class="text-slate-400 text-sm italic">
      No hay tareas todavía. ¡Agrega la primera!
    </p>

    <ul v-else class="divide-y divide-slate-200">
      <li
        v-for="(tarea, index) in tareas"
        :key="index"
        class="flex items-center justify-between py-3"
      >
        <span class="text-slate-700">• {{ tarea }}</span>
        <button
          @click="eliminarTarea(index)"
          class="px-3 py-1 rounded-md bg-red-500 text-white text-xs font-medium hover:bg-red-600 transition-colors"
        >
          ✕ Eliminar
        </button>
      </li>
    </ul>
  </div>
</template>
