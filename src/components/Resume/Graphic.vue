<template>
  <div>
    <svg
      viewBox="0 0 300 200"
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      >
     <!-- Línea horizontal que representa el valor 0 -->
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      />

      <!-- Línea del gráfico (solo se dibuja si hay puntos válidos) -->
      <polyline
        v-if="points"
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="points"
      />

      <!-- Línea vertical que sigue el dedo -->
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />
    </svg>
    <p class="text-white">Últimos 30 días</p>
  </div>
</template>

<script setup>
import {
  ref,
  toRefs,
  computed,
  watch
} from 'vue'



// Recibimos el arreglo de montos
const props = defineProps({
  amounts: {
    type: Array,
    default: () => []
  }
})

// Convertimos los props en referencias reactivas
const { amounts } = toRefs(props)



/*
  SVG tiene una altura de 200.
  Esta función traduce valores numéricos (positivos o negativos)
  a una coordenada Y dentro del gráfico.
*/
const amountToPixels = (amount) => {
  // 🛑 Si no hay datos, devolvemos el centro del gráfico
  if (!amounts.value.length) {
    return 100
  }

  const min = Math.min(...amounts.value)
  const max = Math.max(...amounts.value)

  // 🛑 Si todos los valores son iguales, evitamos divisiones inválidas
  if (min === max) {
    return 100
  }

  // Desplazamos los valores para que el mínimo sea 0
  const amountAbs = amount + Math.abs(min)

  // Distancia total entre el mínimo y el máximo
  const minmax = Math.abs(max) + Math.abs(min)

  // Regla de 3 para llevar el valor al alto del SVG (200px)
  return 200 - ((amountAbs * 100) / minmax) * 2
}



const zero = computed(() => {
  return amountToPixels(0)
})

/*
  Genera el string:
  "x1,y1 x2,y2 x3,y3 ..."
  que necesita el elemento <polyline>.
*/
const points = computed(() => {
  const total = amounts.value.length

  // 🛑 Sin datos → no dibujamos nada
  if (!total) return ''

  return amounts.value.reduce((points, amount, i) => {
    // Posición X proporcional al total de puntos
    const x = (300 / total) * (i + 1)

    // Posición Y calculada según el monto
    const y = amountToPixels(amount)

    return `${points} ${x},${y}`
  }, `0,${amountToPixels(amounts.value[0])}`)
})



// Controla si se muestra la línea vertical
const showPointer = ref(false)

// Posición X de la línea vertical
const pointer = ref(0)

// Emitimos el valor seleccionado
const emit = defineEmits(['select'])

/*
  Cada vez que el usuario mueve el dedo,
  calculamos qué valor del arreglo está señalando.
*/
watch(pointer, (value) => {
  // 🛑 Si no hay datos, no hacemos nada
  if (!amounts.value.length) return

  const step = 300 / amounts.value.length
  const index = Math.ceil(value / step)

  // Protección de límites
  if (index < 1 || index > amounts.value.length) return

  emit('select', amounts.value[index - 1])
})



// Cuando el usuario toca o mueve el dedo
const tap = ({ target, touches }) => {
  showPointer.value = true

  // Ancho real del SVG en pantalla
  const elementWidth = target.getBoundingClientRect().width

  // Posición X inicial del SVG
  const elementX = target.getBoundingClientRect().x

  // Posición X del dedo
  const touchX = touches[0].clientX

  // Regla de 3 para convertir a coordenadas del SVG (300)
  pointer.value = ((touchX - elementX) * 300) / elementWidth
}

// Cuando el usuario levanta el dedo
const untap = () => {
  showPointer.value = false
}
</script>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>
