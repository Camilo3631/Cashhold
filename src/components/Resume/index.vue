<template>
  <main class="flex flex-col items-center justify-center w-full  bg-slate-800 px-4 sm:px-6 md:px-8 lg:px-10 xl:px-12">
     <!-- Contenedor responsivo -->
   <div class="w-full sm:w-11/12 md:w-10/12 lg:w-8/12 mx-auto">

     <!-- Etiqueta -->
   <p class="text-center text-white text-base sm:text-lg md:text-xl lg:text-2xl xl:text-3xl font-medium w-full">
    {{ labelVisual }}
   </p>

    <!-- Cantidad formateada -->
     <h1 class="mt-4 text-center text-white font-bold w-full text-2xl sm:text-2xl md:text-2xl lg:text-2xl xl:text-3xl">
      {{ amountCurrency }}
     </h1>

     <!-- Slot para gráfico -->
    <div class="flex justify-center items-center w-full py-12">
      <slot name="graphic"></slot>
    </div>
   
   <!-- Slot para acciones -->
    <div class="flex justify-center items-center w-full mt-4">
      <slot name="action"></slot>
    </div>

   </div>
  </main>
</template>


<script setup>

import { computed  } from 'vue';

const props = defineProps({
   totalLabel: String,
   label: {
     type: String,
     default: null
   },
   amount: {
     type: Number,
     default: null
   },
   totalAmount: Number
})

const labelVisual = computed(() => props.label?? props.totalLabel)
const amountVisual = computed(() => props.amount ?? props.totalAmount)

const currencyFormatter = new Intl.NumberFormat('es-ES', {
   style: 'currency',
   currency: 'EUR',
})

const amountCurrency = computed(() => currencyFormatter.format(amountVisual.value))
</script>