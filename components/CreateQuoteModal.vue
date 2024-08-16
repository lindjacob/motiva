<template>
  <div class="fixed inset-0 bg-white/90 flex justify-center items-center">
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
          @click="$emit('toggleCreateQuoteModal')"
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
</template>

<script setup>
const { isModalVisible } = defineProps(["isModalVisible"]);
const emit = defineEmits(["update:isModalVisible"]);

const newQuote = ref("");
const newAuthor = ref("");
const isSaveButtonActive = ref(false);

const saveQuote = () => {
  const savedQuotes = JSON.parse(localStorage.getItem("savedQuotes") || "[]");
  savedQuotes.push({ q: newQuote.value, a: newAuthor.value });
  localStorage.setItem("savedQuotes", JSON.stringify(savedQuotes));
  emit("toggleCreateQuoteModal");
};

watch([newQuote, newAuthor], () => {
  isSaveButtonActive.value =
    newQuote.value.length >= 5 && newAuthor.value.length >= 2;
});
</script>

<style scoped></style>
