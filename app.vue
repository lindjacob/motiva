<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <QuoteCard :quote="quotes[quoteNumber]" />
    <div class="mt-15 flex justify-between gap-4">
      <v-btn
        class="w-56"
        size="x-large"
        variant="outlined"
        prepend-icon="mdi-plus-thick"
        @click="showCreateQuoteModal"
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

    <div
      v-if="isModalVisible"
      class="fixed inset-0 bg-white/90 flex justify-center items-center"
    >
      <div class="bg-white p-10 rounded-lg shadow-lg flex flex-col gap-4">
        <v-textarea
          v-model="newQuote"
          color="#5ad796"
          label="Quote"
          rows="2"
        ></v-textarea>
        <v-text-field
          v-model="newAuthor"
          color="#5ad796"
          label="Author"
        ></v-text-field>
        <div class="mt-4 flex justify-between gap-4">
          <v-btn
            class="w-56"
            size="x-large"
            variant="outlined"
            @click="hideCreateQuoteModal"
          >
            Cancel
          </v-btn>
          <v-btn
            class="w-56"
            size="x-large"
            variant="flat"
            color="#5ad796"
            :disabled="!isSaveButtonActive"
            @click="saveQuote"
          >
            Save
          </v-btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from "vue";
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

const isModalVisible = ref(false);
const newQuote = ref("");
const newAuthor = ref("");
const isSaveButtonActive = ref(false);

const showCreateQuoteModal = () => {
  isModalVisible.value = true;
};

const hideCreateQuoteModal = () => {
  isModalVisible.value = false;
  newQuote.value = "";
  newAuthor.value = "";
};

const saveQuote = () => {
  const savedQuotes = JSON.parse(localStorage.getItem("savedQuotes") || "[]");
  savedQuotes.push({ q: newQuote.value, a: newAuthor.value });
  localStorage.setItem("savedQuotes", JSON.stringify(savedQuotes));
  hideCreateQuoteModal();
};

watch([newQuote, newAuthor], () => {
  isSaveButtonActive.value =
    newQuote.value.length >= 5 && newAuthor.value.length >= 2;
});
</script>

<style scoped></style>
