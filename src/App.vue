<script setup>
  import Calculator from './components/Calculator.vue'
</script>

<template>
<header class="w-full bg-blue-900 py-8 text-center text-white">
  <h1 class="text-2xl tracking-widest">52週間貯金チャレンジ</h1>
  <p class="py-2 tracking-widest">今日から始められる貯金習慣</p>
</header>
<main class="w-full h-full py-12">
  <article class="w-11/12 sm:w-7/12 m-auto">
    <Calculator></Calculator>
  </article>
</main>
<footer class="w-full bg-blue-900 py-4 text-center text-white mt-auto">
  <small class="text-base">&copy; 52Week Money charange</small>
</footer>
</template>

<!-- <style scoped>

</style> -->
