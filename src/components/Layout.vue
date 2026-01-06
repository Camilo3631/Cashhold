<template>
  <!-- Header fijo -->
  <div class="header fixed w-full z-40 flex justify-around items-center px-4 sm:px-6 py-3 bg-white shadow-md">
    <slot name="header"></slot>
  </div>

  <!-- Resumen -->
  <div class="resume flex justify-center items-center min-h-screen px-4 sm:px-6 py-3">
    <slot name="resume"></slot>
  </div>

  <!-- Movimientos tipo drawer -->
  <div class="movements fixed bottom-0 w-full z-40 flex flex-col bg-slate-600 shadow-lg rounded-t-3xl">

    <!-- Head con grip (SIN SCROLL) -->
    <div
      class="head flex justify-center items-center p-5 md:p-4 sm:p-6 cursor-pointer"
      @click="showMovements = !showMovements"
    >
      <div class="grip w-20 sm:w-28 h-1 bg-gray-300 rounded"></div>
    </div>

    <!-- Body (AQUÍ va el scroll) -->
    <div
      v-show="showMovements"
      class="
        body w-full
        overflow-y-auto
        max-h-[45vh] sm:max-h-[55vh] md:max-h-[60vh]
        scrollbar-thin
        scrollbar-thumb-slate-400
        scrollbar-track-transparent
      "
      style="scrollbar-width: thin;"
    >
      <slot name="movements"></slot>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'
const showMovements = ref(false)
</script>
