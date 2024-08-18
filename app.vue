<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="currentQuote" />
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
        @click="setNewQuote"
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

// Fetch quotes data
const { data: quotes } = await useFetch("https://zenquotes.io/api/quotes/");

// Reactive state for the current quote
const currentQuote = ref(null);

// Interval to change the quote every 60 seconds
let interval;

// Function to set a new random quote
const setNewQuote = () => {
  const randomIndex = Math.floor(Math.random() * quotes.value.length);
  currentQuote.value = quotes.value[randomIndex];
};

// Set the initial quote on mount
onMounted(() => {
  const storedValue = JSON.parse(
    window.localStorage.getItem("savedQuotes") || "[]"
  );
  if (storedValue) {
    quotes.value = [...quotes.value, ...storedValue];
  }
  setNewQuote();
  interval = setInterval(setNewQuote, 60000); // Change quote every 60 seconds
});

// Clear interval on unmount
onUnmounted(() => {
  clearInterval(interval);
});

// Toggle modals
const isCreateQuoteVisible = ref(false);
const isQuoteListVisible = ref(false);
const toggleCreateQuoteModal = () => {
  isCreateQuoteVisible.value = !isCreateQuoteVisible.value;
};
const toggleQuoteListModal = () => {
  isQuoteListVisible.value = !isQuoteListVisible.value;
};

// Add new quote
const addNewQuote = (newQuote) => {
  quotes.value.push(newQuote);
};
</script>

<style scoped></style>
