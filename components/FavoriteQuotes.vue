<template>
  <v-dialog max-width="1000px">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        icon="mdi-book-outline"
        variant="outlined"
        class="!size-14"
        size="x-large"
        @click="syncFavoriteQuotes"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-sheet>
        <v-data-table-virtual
          :headers="headers"
          :items="favoriteQuotes"
          :sort-by="[{ key: 'q', order: 'asc' }]"
          height="600"
        >
          <template v-slot:top>
            <v-toolbar flat class="pr-5">
              <v-toolbar-title>Favorite Quotes</v-toolbar-title>
            </v-toolbar>
          </template>
        </v-data-table-virtual>
        <v-toolbar flat class="pr-5">
          <v-spacer />
          <v-btn
            variant="flat"
            color="black"
            rounded="xl"
            @click="isActive.value = false"
          >
            Close
          </v-btn>
        </v-toolbar>
      </v-sheet>
    </template>
  </v-dialog>
</template>

<script setup>
import { ref } from "vue";

const favoriteQuotes = ref([]);

if (typeof window !== "undefined") {
  favoriteQuotes.value = JSON.parse(
    localStorage.getItem("favoriteQuotes") || "[]"
  );
}

const headers = ref([
  { title: "Quote", value: "q", sortable: true },
  { title: "Author", value: "a", sortable: true },
]);

const syncFavoriteQuotes = () => {
  favoriteQuotes.value = JSON.parse(
    localStorage.getItem("favoriteQuotes") || "[]"
  );
};
</script>
