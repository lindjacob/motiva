<template>
  <div
    class="flex flex-col items-center justify-center h-screen container mx-auto max-w-[1000px]"
  >
    <QuoteCard :quote="currentQuote" />
    <v-divider opacity="100" class="w-full mt-12" />
    <div class="w-full flex mt-7 justify-between">
      <div class="flex justify-end gap-4">
        <EditQuotes :quotes="quotes">
          <template v-slot:activator>
            <CreateQuote v-on:update:quotes="addNewQuote" />
          </template>
        </EditQuotes>
        <FavoriteQuote :currentQuote="currentQuote" />
        <FavoriteQuotes />
      </div>
      <div class="flex justify-center items-center gap-6">
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

// Favorite quote
const favoriteQuote = () => {
  const favoriteQuotes = JSON.parse(
    localStorage.getItem("favoriteQuotes") || "[]"
  );
  favoriteQuotes.push(currentQuote.value);
  localStorage.setItem("favoriteQuotes", JSON.stringify(favoriteQuotes));
};
</script>

<style scoped></style>
