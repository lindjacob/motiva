<template>
  <blockquote class="text-6xl font-bold text-[#5ad796] max-w-[1000px]">
    &ldquo;{{ quote?.q || "Quotes loading..." }}&rdquo;
    <footer class="mt-5 text-5xl text-black font-normal">
      <p v-if="quote?.a">&mdash; {{ quote?.a }}</p>
      <p v-else></p>
    </footer>
  </blockquote>
</template>

<script setup>
const { quote } = defineProps(["quote"]);
</script>

<style scoped></style>
