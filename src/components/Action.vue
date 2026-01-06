<template>
  <!--Botón principal para abrir la modal -->
  <button
    @click="showModal = true"
    class="w-full sm:w-auto px-6 py-3 rounded-full bg-red-700 text-white text-lg sm:text-xl font-semibold hover:bg-red-600">
    
      Agregar Movimiento
    </button>

    <!-- Modal -->
     <Teleport to="#app">
        <Modal v-show="showModal" @close="showModal = false">
          <form
            @submit.prevent="submit"
            class="w-full max-w-lg mx-auto p-4 sm:p-6 bg-slate-600 rounded-lg text-white sm:rounded-xl"
            >
                    
                    
            <!-- Título -->
             <div class="field flex flex-col mb-1">
              <label class="mb-1 font-medium">Título</label>
              <input
                type="text"
                v-model="title"
                class="border-2 border-red-700 rounded-lg p-2 text-base sm:text-lg text-white focus:outline-none focus:ring-red-700"
                />
             </div>

             <!-- Monto -->
              <div class="field flex flex-col mb-4">
                <label class="mb-1 font-medium">Monto</label>
                <input 
                  type="number"
                  v-model="amount"
                  class="border-2 border-red-700 rounded-lg p-2 text-right text-base sm:text-lg text-white focus:outline-none focus:ring-2 focus:ring-red-700"
                  />
              </div>

               <!-- Descripción -->
               <div class="field flex flex-col mb-4">
                <label class="mb-1 font-medium">Descripción</label>
                <textarea
                   rows="2"
                   v-model="description"
                   class="border-2 border-red-700 rounded-lg p-2 text-base sm:text-lg text-white focus:outline-none focus:ring-2 focus:ring-red-700"
                   ></textarea>
               </div>

                <!-- Tipo de movimiento -->
                <div class="field flex flex-col mb-4">
                  <label  class="mb-2 font-medium">Tipo de movimiento</label>
                  <div class="flex flex-row gap-4">
                    <label class="flex items-center  gap-2">
                      <input 
                        type="radio"
                        v-model="movementType"
                        value="Ingreso"
                        class="w-5 h-5 text-red-700 border-2 border-red-700 rounded-full checked:bg-red-700"
                        />
                        <span class="text-white">Ingreso</span>
                    </label>
                    <label class="flex items-center gap-2">
                      <input
                        type="radio"
                        v-model="movementType"
                        value="Gasto"
                        class="w-5 h-5 text-red-700 border-2 border-red-700 rounded-full checked:bg-red-700"
                        >
                        <span class="text-white">Gasto</span>
                    </label>
                  </div>
                </div>

                 <!-- Botón Agregar -->
                 <div class="flex mt-2 justify-center">
                  <button
                    type="submit"
                    class="w-full text-center sm:w-auto px-3 py-2 rounded-full bg-red-700 text-white text-lg sm:text-xl font-semibold hover:bg-red-600">
                    Agregar Movimiento
                </button>
              </div>
          </form>
      </Modal>
   </Teleport>
</template> 

<script setup>
import { ref} from 'vue'
import Modal from './Modal.vue'

const showModal = ref(false);
const title = ref('');
const amount = ref(0);
const description = ref('');
const movementType = ref('Ingreso');

const emit = defineEmits(['create'])

const submit = () => {
  showModal.value = false;
  emit('create', {
   id: Date.now().toString(),
   title: title.value,
   description: description.value,
   amount: 
     movementType.value === 'Ingreso'
     ?  amount.value
     : -amount.value,
    time: new Date(),
  });

  title.value = '';
  description.value = '';
  amount.value = 0;
  movementType.value = 'Ingreso';
}

</script>