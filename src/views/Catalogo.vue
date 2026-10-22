<script setup>
import { ref } from 'vue'
import { productos } from '../data/mockData.js'
import DetalleProducto from '../components/DetalleProducto.vue'

const productoSeleccionado = ref(null)

function seleccionarProducto(item) {
  productoSeleccionado.value = item
}

function limpiarSeleccion() {
  productoSeleccionado.value = null
}
</script>

<template>
  <div>
    <h2 class="text-xl font-bold text-slate-800 mb-4">Catálogo de Electrónicos</h2>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div class="md:col-span-2 grid grid-cols-2 sm:grid-cols-3 gap-4 content-start">
        <div
          v-for="item in productos"
          :key="item.id"
          class="bg-white rounded-lg shadow p-4 text-center flex flex-col items-center"
        >
          <div class="text-4xl mb-2">{{ item.icono }}</div>
          <p class="text-sm font-medium text-slate-700 truncate w-full">{{ item.nombre }}</p>
          <p class="text-slate-500 text-sm mb-3">${{ item.precio.toFixed(2) }}</p>
          <button
            @click="seleccionarProducto(item)"
            class="mt-auto w-full py-1.5 rounded-md bg-blue-600 text-white text-xs font-medium hover:bg-blue-700 transition-colors"
          >
            Ver Detalles
          </button>
        </div>
      </div>

      <div class="md:col-span-1">
        <DetalleProducto
          v-if="productoSeleccionado"
          :producto="productoSeleccionado"
          @cerrar="limpiarSeleccion"
        />
        <div v-else class="bg-white rounded-lg shadow p-6 text-center text-slate-400 text-sm">
          Selecciona un producto para ver el detalle.
        </div>
      </div>
    </div>
  </div>
</template>
