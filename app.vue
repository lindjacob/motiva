<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="quotes[quoteNumber]" />
    <CreateQuoteModal
      v-if="isCreateQuoteVisible"
      :isCreateQuoteVisible.sync="isCreateQuoteVisible"
      v-on:toggleCreateQuoteModal="toggleCreateQuoteModal"
      v-on:update:quotes="addNewQuote"
    />
    <QuoteList
      v-if="isQuoteListVisible"
      :quotes="quotes"
      v-on:toggleQuoteListModal="toggleQuoteListModal"
    />
    <div class="mt-15 flex justify-between gap-4 mb-4">
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
    <v-btn
      class="w-44"
      size="large"
      variant="outlined"
      append-icon="mdi-eye"
      @click="toggleQuoteListModal"
    >
      View Quotes
    </v-btn>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const { data: quotes } = await useFetch("https://zenquotes.io/api/quotes/");

onMounted(() => {
  const storedValue = JSON.parse(
    window.localStorage.getItem("savedQuotes") || "[]"
  );
  if (storedValue) {
    quotes.value = [...quotes.value, ...storedValue];
  }
});

const quoteNumber = ref(Math.floor(Math.random() * quotes.value.length));
const isCreateQuoteVisible = ref(false);
const isQuoteListVisible = ref(false);

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
  interval = setInterval(getNewQuote, 60000);
});

onUnmounted(() => {
  clearInterval(interval);
});

const toggleCreateQuoteModal = () => {
  isCreateQuoteVisible.value = !isCreateQuoteVisible.value;
};

const toggleQuoteListModal = () => {
  isQuoteListVisible.value = !isQuoteListVisible.value;
};

const addNewQuote = (newQuote) => {
  quotes.value.push(newQuote);
};
</script>

<style scoped></style>
