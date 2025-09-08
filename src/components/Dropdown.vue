<script setup>
import { ref, computed, defineProps, defineEmits, watch } from "vue";
import DropdownSelection from "@/components/DropdownSelection.vue";

const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
  multiselect: {
    type: Boolean,
    default: false,
    required: false,
  },
  placeholder: {
    type: String,
    default: "Виберіть...",
    required: false,
  },
});

const emit = defineEmits(["update:modelValue"]);

const searchQuery = ref("");
const isOpen = ref(false);
const highlightedIndex = ref(-1);
const selectedItems = ref(props.multiselect ? [] : null);

const filteredItems = computed(() => {
  if (searchQuery.value === "") {
    return props.items;
  }

  const searchLowercase = searchQuery.value.toLowerCase();

  return props.items.filter((item) =>
    item.label.toLowerCase().includes(searchLowercase)
  );
});

const selectionLabel = computed(() => {
  if (selectedItems.value !== null && !Array.isArray(selectedItems.value)) {
    return selectedItems.value.label;
  }

  return selectedItems.value
    .map((selectedItem) => selectedItem.label)
    .join(", ");
});

function toggleDropdown() {
  isOpen.value = !isOpen.value;
  if (isOpen.value) searchQuery.value = "";
}

function selectItem(item) {
  if (!props.multiselect) {
    selectedItems.value = item;
    isOpen.value = false;
  } else {
    handleMultiSelect(item);
  }

  emit("update:modelValue", selectedItems.value);
}

function handleMultiSelect(item) {
  const isItemSelected = selectedItems.value.find(
    (selectedItem) => selectedItem.value === item.value
  );

  if (!isItemSelected) {
    selectedItems.value.push(item);
    isOpen.value = false;
  }
}

function clearSelection() {
  selectedItems.value = props.multiselect ? [] : null;
  isOpen.value = false;
  emit("update:modelValue", selectedItems.value);
}

function removeItem(item) {
  selectedItems.value = selectedItems.value.filter(
    (selectedItem) => selectedItem.value !== item.value
  );
  emit("update:modelValue", selectedItems.value);
}
</script>

<template>
  <div class="relative w-64">
    <!-- Selected items -->
    <div
      v-if="
        selectedItems && (!Array.isArray(selectedItems) || selectedItems.length > 0)
      "
      class="mb-2 flex flex-wrap gap-2"
    >
      <DropdownSelection
        v-if="!Array.isArray(selectedItems)"
        :label="selectionLabel"
        @remove="clearSelection"
      />
      <DropdownSelection
        v-else
        v-for="item in selectedItems"
        :key="item.value"
        :label="item.label"
        @remove="removeItem(item)"
      />
    </div>

    <!-- Trigger input -->
    <div class="relative">
      <input
        type="text"
        v-model="searchQuery"
        :placeholder="placeholder"
        @focus="isOpen = true"
        class="block w-full p-2 ps-3 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500"
      />
      <span
        v-if="(!props.multiselect && selectedItems !== null) || (props.multiselect && selectedItems.length > 0)"
        class="absolute text-blue-600 font-medium hover:cursor-pointer hover:text-blue-700 transition-colors duration-300 right-2 top-1/2 transform -translate-y-1/2"
        @click="clearSelection"
      >
        Очистити
      </span>
    </div>

    <!-- Dropdown list -->
    <div
      v-if="isOpen"
      class="absolute left-0 right-0 mt-1 bg-white rounded-lg shadow-sm z-10"
    >
      <ul class="max-h-48 overflow-y-auto text-sm text-gray-700">
        <li
          v-for="(item, index) in filteredItems"
          class="flex items-center ps-2 rounded-sm hover:bg-gray-100 hover:cursor-pointer"
          :class="{
            'bg-blue-100': index === highlightedIndex,
          }"
          :key="item.value"
          @click="selectItem(item)"
        >
          <span
            class="w-full py-2 ms-2 text-sm font-medium text-gray-900 rounded-sm"
          >
            {{ item.label }}
          </span>
        </li>
        <li v-if="filteredItems.length === 0" class="px-3 py-2 text-gray-400">
          Нічого не знайдено
        </li>
      </ul>
    </div>
  </div>
</template>
