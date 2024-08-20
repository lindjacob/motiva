<template>
  <v-dialog max-width="1000px">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        text="Edit Quotes"
        variant="outlined"
        prepend-icon="mdi-pencil"
        rounded="xl"
        size="x-large"
        class="w-52"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-sheet>
        <v-data-table-virtual
          :headers="headers"
          :items="quotes"
          :sort-by="[{ key: 'q', order: 'asc' }]"
          height="600"
        >
          <template v-slot:top>
            <v-toolbar flat class="pr-5">
              <v-toolbar-title>Quotes</v-toolbar-title>
              <v-spacer />
              <slot name="activator"></slot>
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
import { ref, reactive, computed, watch, onMounted } from "vue";

const { quotes } = defineProps(["quotes"]);
const emit = defineEmits(["toggleQuoteListModal"]);

const dialog = ref(false);
const dialogDelete = ref(false);
const headers = ref([
  { title: "Quote", value: "q", sortable: true },
  { title: "Author", value: "a", sortable: true },
]);

const editedIndex = ref(-1);
const editedItem = reactive({
  q: "",
  a: "",
});
const defaultItem = {
  name: "",
  a: "",
};

const formTitle = computed(() =>
  editedIndex.value === -1 ? "New Item" : "Edit Item"
);

watch(dialog, (val) => {
  if (!val) close();
});
watch(dialogDelete, (val) => {
  if (!val) closeDelete();
});

function editItem(item) {
  editedIndex.value = desserts.value.indexOf(item);
  Object.assign(editedItem, item);
  dialog.value = true;
}

function deleteItem(item) {
  editedIndex.value = desserts.value.indexOf(item);
  Object.assign(editedItem, item);
  dialogDelete.value = true;
}

function deleteItemConfirm() {
  desserts.value.splice(editedIndex.value, 1);
  closeDelete();
}

function close() {
  dialog.value = false;
  nextTick(() => {
    Object.assign(editedItem, defaultItem);
    editedIndex.value = -1;
  });
}

function closeDelete() {
  dialogDelete.value = false;
  nextTick(() => {
    Object.assign(editedItem, defaultItem);
    editedIndex.value = -1;
  });
}

function save() {
  if (editedIndex.value > -1) {
    Object.assign(desserts.value[editedIndex.value], editedItem);
  } else {
    desserts.value.push({ ...editedItem });
  }
  close();
}
</script>
