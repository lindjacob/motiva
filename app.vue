<template>
  <div
    class="flex flex-col items-center justify-center h-screen container mx-auto max-w-[1000px] px-5 sm:px-20"
  >
    <QuoteCard :quote="currentQuote" />
    <v-divider opacity="100" class="w-full mt-12" />
    <div
      class="w-full mt-7 flex flex-col gap-5 items-center sm:flex-row sm:justify-between sm:gap-0"
    >
      <div class="flex justify-end gap-1 md:gap-4">
        <EditQuotes :quotes="quotes">
          <template v-slot:activator>
            <CreateQuote v-on:update:quotes="addNewQuote" />
          </template>
        </EditQuotes>
        <FavoriteQuote :currentQuote="currentQuote" />
        <FavoriteQuotes />
      </div>
      <v-btn
        class="w-52 !border-[#5ad796]"
        size="x-large"
        variant="flat"
        rounded="xl"
        color="#5ad796"
        append-icon="mdi-auto-fix"
        @click="setNewQuote"
      >
        Inspire Me!
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
