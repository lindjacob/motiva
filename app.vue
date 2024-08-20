<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="currentQuote" />
    <div class="flex justify-center items-center gap-6 mt-12">
      <EditQuotes class="mt-40" :quotes="quotes">
        <template v-slot:activator>
          <CreateQuote v-on:update:quotes="addNewQuote" />
        </template>
      </EditQuotes>
      <v-btn
        class="w-52 !border-[#5ad796]"
        size="x-large"
        variant="outlined"
        rounded="xl"
        append-icon="mdi-fast-forward"
        @click="setNewQuote"
      >
        New Quote
      </v-btn>
    </div>
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

// Add new quote
const addNewQuote = (newQuote) => {
  quotes.value.push(newQuote);
};
</script>

<style scoped></style>
