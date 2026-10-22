<script setup>
defineProps({
  producto: Object,
})

const emit = defineEmits(['cerrar'])
</script>

<template>
  <div class="bg-white rounded-lg shadow p-6 sticky top-4">
    <div class="text-7xl text-center mb-4">{{ producto.icono }}</div>

    <h3 class="text-lg font-bold text-slate-800 mb-2">{{ producto.nombre }}</h3>

    <p class="text-sm text-slate-500 mb-1">Descripción:</p>
    <p class="text-slate-600 mb-4">{{ producto.descripcion }}</p>

    <p class="text-2xl font-bold text-cyan-600 mb-6">${{ producto.precio.toFixed(2) }}</p>

    <button
      @click="emit('cerrar')"
      class="w-full py-2 rounded-md bg-slate-800 text-white font-medium hover:bg-slate-900 transition-colors"
    >
      Cerrar
    </button>
  </div>
</template>
