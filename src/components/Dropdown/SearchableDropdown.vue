<script setup>
import DropdownContent from "@/components/Dropdown/DropdownContent.vue";
import DropdownItem from "@/components/Dropdown/DropdownItem.vue";
import DropdownItemNotFound from "@/components/Dropdown/DropdownItemNotFound.vue";
import DropdownList from "@/components/Dropdown/DropdownList.vue";
import DropdownSelection from "@/components/Dropdown/DropdownSelection.vue";
import DropdownTrigger from "@/components/Dropdown/DropdownTrigger.vue";
import { computed, ref, watch } from "vue";

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
  customItemClass: {
    type: String,
    default: "",
    required: false,
  }
});

const emit = defineEmits(["update:modelValue"]);

const isOpen = ref(false);

// Handle items filtering
const searchQuery = ref("");

const filteredItems = computed(() => {
  if (searchQuery.value === "") {
    return props.items;
  }

  const searchLowercase = searchQuery.value.toLowerCase();

  return props.items.filter((item) =>
    item.label.toLowerCase().includes(searchLowercase)
  );
});

// Handle item selection
const selectedItems = ref(props.multiselect ? [] : null);

const selectionLabel = computed(() => {
  if (selectedItems.value !== null && !Array.isArray(selectedItems.value)) {
    return selectedItems.value.label;
  }

  return selectedItems.value
    .map((selectedItem) => selectedItem.label)
    .join(", ");
});

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

// Handle keyboard navigation
const currentItemIndex = ref(null);

function moveDown() {
  if (currentItemIndex.value === null) {
    currentItemIndex.value = 0;
    return;
  }

  if (currentItemIndex.value === filteredItems.value.length - 1) {
    return;
  }

  currentItemIndex.value += 1;
}

function moveUp() {
  if (currentItemIndex.value === null) {
    currentItemIndex.value = filteredItems.value.length - 1;
    return;
  }

  if (currentItemIndex.value === 0) {
    return;
  }

  currentItemIndex.value -= 1;
}

function handleEnterClicked() {
  isOpen.value = !isOpen.value;

  if (currentItemIndex.value !== null) {
    selectItem(filteredItems.value[currentItemIndex.value]);
  }
}

watch(isOpen, () => {
  if (!isOpen.value) {
    searchQuery.value = "";
    currentItemIndex.value = null;
  }
});
</script>

<template>
  <div
    class="relative w-64"
    tabindex="0"
  >
    <!-- Selected items -->
    <div
      v-if="
        selectedItems &&
        (!Array.isArray(selectedItems) || selectedItems.length > 0)
      "
      class="mb-2 flex flex-wrap gap-2"
    >
      <DropdownSelection
        v-if="!Array.isArray(selectedItems)"
        class="w-full justify-between"
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

    <DropdownTrigger
      v-model="searchQuery"
      :is-open="isOpen"
      :placeholder="props.placeholder"
      :showClearButton="
        (!props.multiselect && selectedItems !== null) ||
        (props.multiselect && selectedItems.length > 0)
      "
      @click="isOpen = !isOpen"
      @clear-selection="clearSelection"
      @arrow-up="moveUp"
      @arrow-down="moveDown"
      @enter="handleEnterClicked"
    />

    <!-- Dropdown list -->
    <DropdownContent :is-open="isOpen">
      <DropdownList>
        <DropdownItem
          class="m-0.5"
          :class="[{
            'bg-gray-100 outline outline-blue-500': index === currentItemIndex,
          }, props.customItemClass]"
          v-for="(item, index) in filteredItems"
          :key="index"
          :label="item.label"
          @click.stop="selectItem(item)"
        />
        <DropdownItemNotFound v-if="filteredItems.length === 0" />
      </DropdownList>
    </DropdownContent>
  </div>
</template>
