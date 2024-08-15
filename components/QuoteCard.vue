<template>
  <blockquote class="text-6xl font-bold text-[#5ad796] max-w-[1000px]">
    &ldquo; {{ quote.q }} &rdquo;
    <footer class="mt-8 text-5xl text-black font-normal">
      <p>&mdash; {{ quote.a }}</p>
    </footer>
  </blockquote>
</template>

<script setup>
const { quote } = defineProps(["quote"]);
</script>

<style scoped></style>
