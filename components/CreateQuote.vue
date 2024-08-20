<template>
  <v-dialog max-width="600px">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        color="#5ad796"
        text="Create Quote"
        variant="flat"
        prepend-icon="mdi-plus-thick"
        rounded="xl"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card title="Create Quote">
        <v-card-text>
          <v-textarea v-model="newQuote" label="Quote" rows="2"></v-textarea>
          <v-text-field v-model="newAuthor" label="Author"></v-text-field>

          <div class="mt-4 flex justify-between gap-4">
            <v-btn
              class="w-56"
              size="x-large"
              variant="outlined"
              @click="isActive.value = false"
            >
              Cancel
            </v-btn>
            <v-btn
              class="w-56"
              size="x-large"
              variant="flat"
              color="#5ad796"
              :disabled="!isSaveButtonActive"
              @click="
                saveQuote();
                isActive.value = false;
              "
            >
              Save
            </v-btn>
          </div>
        </v-card-text>
      </v-card>
    </template>
  </v-dialog>
</template>

<script setup>
const emit = defineEmits(["update:quotes"]);

const newQuote = ref("");
const newAuthor = ref("");
const isSaveButtonActive = ref(false);

const saveQuote = () => {
  const newQuoteData = { q: newQuote.value, a: newAuthor.value };
  const savedQuotes = JSON.parse(localStorage.getItem("savedQuotes") || "[]");
  savedQuotes.push(newQuoteData);
  localStorage.setItem("savedQuotes", JSON.stringify(savedQuotes));
  newQuote.value = "";
  newAuthor.value = "";
  emit("update:quotes", newQuoteData);
};

watch([newQuote, newAuthor], () => {
  isSaveButtonActive.value =
    newQuote.value.length >= 5 && newAuthor.value.length >= 2;
});
</script>
