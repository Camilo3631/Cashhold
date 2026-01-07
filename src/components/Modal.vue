<template>
  <!-- Overlay -->
  <div
    v-if="showModal"
    class="fixed inset-0 z-40 flex items-start sm:items-center justify-center bg-black/50 px-2"
  >
    <!-- Modal -->
    <div
      class="
        w-full
        max-w-[360px]
        sm:max-w-sm
        md:max-w-md

        /* 📱 móvil (px) */
        max-h-[520px]
        mt-8
        mb-8

        /* 📲 tablet + pc (vh) */
        sm:max-h-[79vh]
        sm:mt-9
        sm:mb-2

        rounded-lg
        shadow-md
        bg-slate-600
        flex
        flex-col
      "
    >
      <!-- Header -->
      <div class="flex justify-between items-center px-3 py-2 border-b border-gray-300 shrink-0">
        <p class="text-sm font-semibold text-white">
          Nuevo movimiento
        </p>

        <img
          @click="close"
          src="@/assets/close-icon.svg"
          alt="cerrar"
          class="w-4 h-4 cursor-pointer"
        />
      </div>

      <!-- Body -->
      <div class="px-3 py-3 overflow-y-auto">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineEmits } from 'vue'

const emit = defineEmits(['close'])
const showModal = ref(false)

const checkVisibility = () => {
  const width = window.innerWidth
  const height = window.innerHeight

  const isPortrait = height >= width
  const isMobile = Math.min(width, height) < 768

  if (isMobile) {
    showModal.value = isPortrait && width >= 340
  } else {
    showModal.value = true
  }
}

onMounted(() => {
  checkVisibility()
  window.addEventListener('resize', checkVisibility)
})

onUnmounted(() => {
  window.removeEventListener('resize', checkVisibility)
})

const close = () => emit('close')
</script>
