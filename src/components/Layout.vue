<template>
  <div class="min-h-screen w-full flex flex-col">

    <header class="w-full fixed top-0 left-0 z-50 bg-white">
      <slot name="header"></slot>
    </header>

    <!-- Main ocupa todo el ancho y altura restante -->
    <main class="w-full flex flex-col gap-10 px-5 pt-24 h-screen overflow-hidden">

      <!-- Resumen centrado horizontalmente -->
      <section class="w-full flex justify-center">
        <div class="w-full max-w-3xl">
          <slot name="resume"></slot>
        </div>
      </section>

      <!-- Movimientos centrados horizontalmente -->
      <section class="w-full flex justify-center">
        <div class="w-full max-w-3xl">
          <slot name="movements"></slot>
        </div>
      </section>  

    </main>
  </div>
</template>

<style scoped>
header {
  height: 6rem; /* pt-24 = 6rem */
}
</style>
