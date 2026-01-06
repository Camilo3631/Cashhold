<template>
  <Layout>
    <template #header>
      <Header />
    </template>

    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @select="select " />
        </template>
        <template #action>
          <Action @create="create" />
        </template>
      </Resume>
    </template>

    <template #movements>
      <Movements :movements="movements" @remove="remove" />
    </template>
  </Layout>
</template>

<script setup>
import { ref, computed,  onMounted } from "vue";
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/index.vue";
import Action from "./Action.vue";
import Movements from "./Movements/index.vue";
import Graphic from "./Resume/Graphic.vue";

// Variables reactivas
const label = ref(null);
const amount = ref(null);
const movements = ref([
]);

// Computed
const amounts = computed(() => {
  const lastDays = movements.value
    .filter(m => {
      const today = new Date();
      const oldDate = today.setDate(today.getDate() - 30);
      return m.time > oldDate;
    })
    .map(m => m.amount);

  return lastDays.map((m, i) => {
    const lastMovements = lastDays.slice(0, i + 1);
    return lastMovements.reduce((suma, movement) => suma + movement, 0);
  });
});

const totalAmount = computed(() => {
  return movements.value.reduce((suma, m) => suma + m.amount, 0);
});

// Métodos
const create = (movement) => {
  movements.value.push(movement);
  save()
};

const remove = (id) => {
  const index = movements.value.findIndex(m => m.id === id);
  if (index !== -1) movements.value.splice(index, 1);
  save()
};


const save = () => {
  localStorage.setItem("movements", JSON.stringify(movements.value));
}

function select(el) {
  amount.value = el;
}


onMounted(() => {
  const movementsFromLocalStorage = localStorage.getItem("movements");
  if (movementsFromLocalStorage) {
    movements.value = JSON.parse(movementsFromLocalStorage).map(movement => ({
      ...movement,
      time: new Date(movement.time),
    }));
  } else {
    // Si no hay nada en localStorage, inicializamos con un array vacío
    localStorage.setItem("movements", JSON.stringify([]));
  }
});

</script>