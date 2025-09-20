<script setup>
import { library } from "@fortawesome/fontawesome-svg-core";
import { far } from "@fortawesome/free-regular-svg-icons";
import { fas } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(fas, far);

const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  color: {
    type: String,
    default: "blue",
    required: false,
  },
  size: {
    type: String,
    default: "md",
    required: false,
  },
  icon: {
    type: String,
    default: null,
    required: false,
  },
});

const emit = defineEmits(["click"]);

const sizeClasses = {
  xs: "px-2 py-0.5 text-xs",
  sm: "px-3 py-1 text-sm",
  md: "px-4 py-2 text-base",
  lg: "px-6 py-3 text-lg",
  xl: "px-8 py-4 text-2xl",
};

const colorClasses = {
  red: "bg-red-600 hover:bg-red-700 text-white",
  green: "bg-green-600 hover:bg-green-700 text-white",
  blue: "bg-blue-600 hover:bg-blue-700 text-white",
};
</script>

<template>
  <button
    class="flex items-center gap-2 rounded-md font-medium transition-colors duration-300 hover:cursor-pointer shadow-md"
    :class="[sizeClasses[props.size], colorClasses[props.color]]"
    @click="emit('click')"
  >
    <FontAwesomeIcon
      v-if="props.icon !== null"
      :icon="props.icon"
      class="w-4 h-4"
    />
    <span>{{ props.label }}</span>
  </button>
</template>
