<template>
<Suspense>
    <template #default>
       <Home/>
    </template>
    <template #fallback>
        <SplashScreen/> 
    </template>
</Suspense>
</template>







<script setup>
import { defineAsyncComponent } from 'vue'
import SplashScreen from './components/SplashScreen.vue' 

const Home = defineAsyncComponent(() =>
  new Promise((resolve) => {
    setTimeout(() => {
      resolve(import('./components/Home.vue'))
    }, 2500)
  })
)
</script>

