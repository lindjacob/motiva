<template>
  <div class="w-full text-4xl md:text-5xl font-bold text-[#5ad796] text-left">
    <v-icon
      color="#5ad796"
      icon="mdi-format-quote-open"
      size="small"
      class="self-start -mt-8"
    ></v-icon>
    {{ quote?.q || "Quotes loading..." }}
    <v-icon
      color="#5ad796"
      icon="mdi-format-quote-close"
      size="small"
      class="-mb-8"
    ></v-icon>
    <footer class="mt-8 text-3xl md:text-4xl text-black font-normal text-right">
      <p v-if="quote?.a">&mdash; {{ quote?.a }}</p>
      <p v-else></p>
    </footer>
  </div>
</template>

<script setup>
const { quote } = defineProps(["quote"]);
</script>

<style scoped></style>
