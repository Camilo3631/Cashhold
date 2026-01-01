<template>
  <Layout>
    <template #header>
      <Header/>
    </template>

    <template #resume>
     <Resume
     :total-label="'Ahorro total'"
     :label="label"
     :amount="10000"
     :total-amount="amount"
     />
    </template>

    <template #movements>
      <Movements/>
    </template>
  </Layout>
</template>

<script setup>
import { ref } from "vue";
import Layout from "./Layout.vue";
import Header from './Header.vue';
//import Action from "./Action.vue";
import Resume from "./Resume/index.vue";
import Movements from "./Movements/index.vue";

const amount = ref(null);
const label = ref(null);
</script>
