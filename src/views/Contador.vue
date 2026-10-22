<script setup>
import { ref } from 'vue'

const contador = ref(0)

function incrementar() {
  contador.value++
}

function decrementar() {
  contador.value--
}
</script>

<template>
  <div class="max-w-md mx-auto bg-white rounded-lg shadow p-6 text-center">
    <h2 class="text-xl font-bold text-slate-800 mb-6">Ejercicio Contador Interactivo</h2>

    <p class="text-slate-500 text-sm mb-1">Valor Actual:</p>
    <p class="text-5xl font-bold text-cyan-600 mb-6">{{ contador }}</p>

    <div class="flex justify-center gap-4 mb-6">
      <button
        @click="decrementar"
        class="px-5 py-2 rounded-md bg-red-500 text-white font-medium hover:bg-red-600 transition-colors"
      >
        − Decrementar
      </button>
      <button
        @click="incrementar"
        class="px-5 py-2 rounded-md bg-green-600 text-white font-medium hover:bg-green-700 transition-colors"
      >
        + Incrementar
      </button>
    </div>

    <div
      v-if="contador > 10"
      class="bg-red-100 border border-red-300 text-red-700 rounded-md px-4 py-3 text-sm"
    >
      ⚠ ¡Límite superior alcanzado! (El valor supera 10)
    </div>
  </div>
</template>
