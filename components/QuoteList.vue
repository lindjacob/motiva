<template>
  <div class="fixed inset-0 bg-white/90 flex justify-center items-center z-10">
    <div
      class="bg-white p-10 rounded-lg shadow-lg flex flex-col gap-4 h-[800px] text-2"
    >
      <v-card title="" flat>
        <template v-slot:text>
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            variant="outlined"
            hide-details
            single-line
          ></v-text-field>
        </template>
        <v-data-table-virtual
          :headers="[
            { title: 'Quote', value: 'q', sortable: true },
            { title: 'Author', value: 'a', sortable: true },
          ]"
          :items="quotes"
          height="530"
          :search="search"
          hide-default-header
        ></v-data-table-virtual>

        <v-btn
          class="w-56 mt-10"
          size="x-large"
          variant="outlined"
          @click="$emit('toggleQuoteListModal')"
        >
          Close
        </v-btn>
      </v-card>
    </div>
  </div>
</template>

<script setup>
const { quotes } = defineProps(["quotes"]);
const emit = defineEmits(["toggleQuoteListModal"]);
const search = ref("");
</script>

<style scoped></style>
