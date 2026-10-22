<script setup>
import Contador from './views/Contador.vue'
</script>

<template>
  <div class="min-h-screen bg-slate-100">
    <header class="bg-slate-900 text-white shadow">
      <div class="max-w-5xl mx-auto px-4 py-4">
        <h1 class="text-xl font-bold">Práctica N.º 1 - INF 3911 "A"</h1>
        <p class="text-slate-300 text-sm">Ejercicio 1: Contador Interactivo</p>
      </div>
    </header>

    <main class="max-w-5xl mx-auto px-4 py-8">
      <Contador />
    </main>
  </div>
</template>
