<template>
  <!-- Botón principal para abrir la modal -->
  <button
    @click="showModal = true"
    class="px-6 py-2 rounded-full bg-red-700 text-white text-lg sm:text-xl font-semibold hover:bg-red-600">
      Agregar Movimiento
  </button>

  <!-- Modal -->
  <Teleport to="#app">
    <Modal v-show="showModal" @close="showModal = false">
      <form
        @submit.prevent="submit"
        class="w-full max-w-sm sm:max-w-lg
               mx-auto
               p-4 sm:p-6
               bg-slate-600
               rounded-lg sm:rounded-xl
               text-white">

        <!-- Título -->
        <div class="flex flex-col mb-3">
          <label class="mb-1 text-sm font-medium">Título</label>
          <input
            type="text"
            v-model="title"
            class="border-2 border-red-700
                   rounded-md
                   p-2
                   text-sm sm:text-base
                   text-white
                   focus:outline-none
                   focus:ring-2 focus:ring-red-700" />
        </div>

        <!-- Monto -->
        <div class="flex flex-col mb-3">
          <label class="mb-1 text-sm font-medium">Monto</label>
          <input
            type="number"
            v-model="amount"
            class="border-2 border-red-700
                   rounded-md
                   p-2
                   text-sm sm:text-base
                   text-right
                   text-white
                   focus:outline-none
                   focus:ring-2 focus:ring-red-700" />
        </div>

        <!-- Descripción -->
        <div class="flex flex-col mb-3">
          <label class="mb-1 text-sm font-medium">Descripción</label>
          <textarea
            rows="5"
            v-model="description"
            class="border-2 border-red-700
                   rounded-md
                   p-2
                   text-sm sm:text-base
                   text-white
                   focus:outline-none
                   focus:ring-2 focus:ring-red-700">
          </textarea>
        </div>

        <!-- Tipo de movimiento -->
        <div class="flex flex-col mb-4">
          <label class="mb-2 text-sm font-medium">Tipo de movimiento</label>
          <div class="flex gap-4">
            <label class="flex items-center gap-2 text-sm">
              <input
                type="radio"
                v-model="movementType"
                value="Ingreso"
                class="w-4 h-4
                       text-red-700
                       border-red-700
                       focus:ring-red-700" />
              Ingreso
            </label>

            <label class="flex items-center gap-2 text-sm">
              <input
                type="radio"
                v-model="movementType"
                value="Gasto"
                class="w-4 h-4
                       text-red-700
                       border-red-700
                       focus:ring-red-700" />
              Gasto
            </label>
          </div>
        </div>

        <!-- Botón submit -->
        <div class="flex justify-center">
          <button
            type="submit"
            class="px-6 py-2 rounded-full bg-red-700 text-white text-sm sm:text-lg font-semibold hover:bg-red-600 active:scale-[0.98]">
            Agregar Movimiento
          </button>
        </div>
      </form>
    </Modal>
  </Teleport>
</template>

<script setup>
import { ref, watch } from 'vue'
import Modal from './Modal.vue'

const showModal = ref(false)
const title = ref('')
const amount = ref(0)
const description = ref('')
const movementType = ref('Ingreso')

const emit = defineEmits(['create'])

// Bloquea scroll del body mientras el modal está abierto
watch(showModal, (val) => {
  document.body.style.overflow = val ? 'hidden' : 'auto';
});

const submit = () => {
  showModal.value = false

  emit('create', {
   id: Date.now().toString(),
   title: title.value,
   description: description.value,
   amount: movementType.value === 'Ingreso' ? amount.value : -amount.value,
   time: new Date(),
  });

  // Reset de campos
  title.value = '';
  description.value = '';
  amount.value = 0;
  movementType.value = 'Ingreso';
}
</script>
