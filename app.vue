<template>
  <div class="container flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="quotes[quoteNumber]" />
    <div class="mt-16 flex justify-between gap-4">
      <v-btn
        class="w-56"
        size="x-large"
        variant="outlined"
        prepend-icon="mdi-plus-thick"
        @click="getNewQuote"
      >
        Create quote
      </v-btn>
      <v-btn
        class="w-56"
        size="x-large"
        variant="outlined"
        append-icon="mdi-fast-forward"
        @click="getNewQuote"
      >
        New Quote
      </v-btn>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
const { data: quotes } = await useFetch("https://zenquotes.io/api/quotes/");
const quoteNumber = ref(Math.floor(Math.random() * quotes.value.length));

let interval;

const getNewQuote = () => {
  if (quoteNumber.value + 1 >= quotes.value.length) {
    quoteNumber.value = 0;
  } else {
    quoteNumber.value++;
  }
  resetTimer();
};

const resetTimer = () => {
  clearInterval(interval);
  interval = setInterval(getNewQuote, 60000); // Change quote every 60 seconds
};

onMounted(() => {
  interval = setInterval(getNewQuote, 60000); // Initial interval setup
});

onUnmounted(() => {
  clearInterval(interval); // Clear interval when component is unmounted
});
</script>

<style scoped></style>
