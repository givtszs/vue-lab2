<script setup>
import { ref } from 'vue';

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true,
    default: false,
  },
  placeholder: {
    type: String,
    default: "Виберіть...",
    required: false,
  },
  showClearButton: {
    type: Boolean,
    default: false,
    required: false,
  },
});

const emit = defineEmits(["focus", "clearSelection", "arrowUp", "arrowDown", "enter"]);

const searchQuery = defineModel();
</script>

<template>
  <div class="relative">
    <input
      tabindex="0"
      class="block w-full p-2 ps-3 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50"
      :class="{'outline outline-blue-500 focus:outline focus:outline-blue-500': props.isOpen, 'caret-transparent': !props.isOpen}"
      type="text"
      v-model="searchQuery"
      :placeholder="props.placeholder"
      @keydown.up="emit('arrowUp', $event)"
      @keydown.down="emit('arrowDown', $event)"
      @keydown.enter="emit('enter', $event)"
    />
    <span
      v-if="props.showClearButton"
      class="absolute text-blue-600 font-medium hover:cursor-pointer hover:text-blue-700 transition-colors duration-300 right-2 top-1/2 transform -translate-y-1/2"
      @click.stop="emit('clearSelection')"
    >
      Очистити
    </span>
  </div>
</template>
