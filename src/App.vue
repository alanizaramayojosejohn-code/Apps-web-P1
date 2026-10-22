<script setup>
import { ref } from 'vue'
import Contador from './views/Contador.vue'
import ListaTareas from './views/ListaTareas.vue'
import Catalogo from './views/Catalogo.vue'

const vistas = [
  { id: 'contador', nombre: '1. Contador', componente: Contador },
  { id: 'tareas', nombre: '2. Lista de Tareas', componente: ListaTareas },
  { id: 'catalogo', nombre: '3. Catálogo', componente: Catalogo },
]

const vistaActiva = ref('contador')
</script>

<template>
  <div class="min-h-screen bg-slate-100">
    <header class="bg-slate-900 text-white shadow">
      <div class="max-w-5xl mx-auto px-4 py-4">
        <h1 class="text-xl font-bold">Práctica N.º 1 - INF 3911 "A"</h1>
        <p class="text-slate-300 text-sm">Ejercicios prácticos con Vue 3 (Composition API)</p>
      </div>
    </header>

    <nav class="bg-white border-b shadow-sm">
      <div class="max-w-5xl mx-auto px-4 flex gap-2">
        <button
          v-for="vista in vistas"
          :key="vista.id"
          @click="vistaActiva = vista.id"
          class="px-4 py-3 text-sm font-medium border-b-2 transition-colors"
          :class="vistaActiva === vista.id
            ? 'border-blue-600 text-blue-600'
            : 'border-transparent text-slate-500 hover:text-slate-800'"
        >
          {{ vista.nombre }}
        </button>
      </div>
    </nav>

    <main class="max-w-5xl mx-auto px-4 py-8">
      <component :is="vistas.find(v => v.id === vistaActiva).componente" />
    </main>
  </div>
</template>
