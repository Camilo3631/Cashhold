
<template>
  <div class="movement flex justify-between items-center w-full p-3 bg-slate-600 rounded-lg box-border">
    <!-- Contenido -->
    <div class="content flex-1">
      <h4 class="text-lg sm:text-xl md:text-2xl font-semibold mb-2 text-white">
        {{ title }}
      </h4>
      <p class="text-gray-200 text-sm sm:text-base md:text-lg">{{ description }}</p>
    </div>

    <!-- Acción -->
    <div class="action flex flex-col items-end">
      <img
        src="@/assets/trash-icon.svg"
        alt="borrar"
        class="mb-4 w-6 h-6 sm:w-7 sm:h-7 md:w-8 md:h-8 cursor-pointer hover:opacity-80"
        @click="remove"
      />
      <p :class="amountClass" class="text-base sm:text-lg md:text-xl font-semibold">
        {{ amountCurrency }}
      </p>
    </div>
  </div>
</template>

<script setup>
import { computed,  toRefs } from 'vue';

const currencyFormatter = new Intl.NumberFormat('es-ES', {
  style: 'currency',
  currency: 'EUR',
});

const props = defineProps({
  id: String,
  title: String,
  description: String,
  amount: Number,
});

const { id, title, description, amount } = toRefs(props);

const emits = defineEmits(['remove']);

const amountCurrency = computed(() => currencyFormatter.format(amount.value));

// Tailwind classes dinámicas
const amountClass = computed(() => (amount.value < 0 ? 'text-red-500' : 'text-green-500'));

const remove = () => emits('remove', id.value);
</script>
