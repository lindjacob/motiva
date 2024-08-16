<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="quotes[quoteNumber]" />
    <CreateQuoteModal
      v-if="isModalVisible"
      :isModalVisible.sync="isModalVisible"
      v-on:toggleCreateQuoteModal="toggleCreateQuoteModal"
    />
    <div class="mt-15 flex justify-between gap-4">
      <v-btn
        class="w-56"
        size="x-large"
        variant="outlined"
        prepend-icon="mdi-plus-thick"
        @click="toggleCreateQuoteModal"
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
import { ref, onMounted, onUnmounted, watch } from "vue";
const { data: quotes } = await useFetch("https://zenquotes.io/api/quotes/");
const quoteNumber = ref(Math.floor(Math.random() * quotes.value.length));
const isModalVisible = ref(false);

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

const toggleCreateQuoteModal = () => {
  isModalVisible.value = !isModalVisible.value;
};
</script>

<style scoped></style>
