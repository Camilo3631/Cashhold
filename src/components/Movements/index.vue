<template>
  <div class="movements px-2 sm:px-4 md:px-6 lg:px-8">
    <h2 class="text-white  text-xl sm:text-2xl md:text-3xl font-semibold my-4">
      Historial
    </h2>

    <div class="content flex flex-col gap-2 sm:gap-4 overflow-y-auto max-h-[60vh] sm:max-h-[65vh] md:max-h-[70vh] lg:max-h-[75vh]">
      <Movement
        v-for="{id, title, description, amount} in movements"
        :key="id"
        :id="id"
        :title="title"
        :description="description"
        :amount="amount"
        @remove="remove"
        />
    </div>
  </div>
</template>

<script setup>
  import { toRefs } from 'vue';
  import Movement from './Movement.vue';

  const props = defineProps({
    movements: {
      type: Array,
      default: () => [],
    },
  })

const { movements} = toRefs(props)

const emit = defineEmits(['remove'])

const remove = (id) => {
  emit('remove', id)
}
</script>