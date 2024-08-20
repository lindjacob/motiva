<template>
  <v-btn
    aria-label="Favorite Quote"
    :icon="isFavorite ? 'mdi-bookmark' : 'mdi-bookmark-outline'"
    variant="outlined"
    size="x-large"
    class="!size-14"
    @click="toggleFavorite"
  ></v-btn>
</template>

<script setup>
import { ref, watch } from "vue";

const isFavorite = ref(false);

const props = defineProps({
  currentQuote: {
    type: Object,
    default: null,
  },
});

const toggleFavorite = () => {
  const favoriteQuotes = JSON.parse(
    localStorage.getItem("favoriteQuotes") || "[]"
  );
  if (!isFavorite.value) {
    favoriteQuotes.push(props.currentQuote);
  } else {
    const index = favoriteQuotes.findIndex(
      (quote) =>
        quote.q === props.currentQuote.q && quote.a === props.currentQuote.a
    );
    if (index !== -1) {
      favoriteQuotes.splice(index, 1);
    }
  }
  localStorage.setItem("favoriteQuotes", JSON.stringify(favoriteQuotes));
  isFavorite.value = !isFavorite.value;
};

// Update isFavorite
watch(
  () => props.currentQuote,
  (currentQuote) => {
    console.log("currentQuote:", currentQuote);
    if (currentQuote) {
      const favoriteQuotes = JSON.parse(
        localStorage.getItem("favoriteQuotes") || "[]"
      );
      isFavorite.value = favoriteQuotes.some(
        (quote) => currentQuote.q === quote.q && currentQuote.a === quote.a
      );
      console.log(isFavorite.value);
    }
  }
);
</script>

<style scoped></style>
