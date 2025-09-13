<script setup>
const props = defineProps({
    label: {
        type: String,
        default: "",
        required: true,
    },
});

const emit = defineEmits(['remove']);
</script>

<template>
  <span class="inline-flex items-center px-2 py-1 text-sm font-medium text-blue-800 bg-blue-100 rounded dark:bg-blue-900 dark:text-blue-200">
    {{ props.label }}
    <button
      @click.stop="emit('remove')"
      class="ml-1 text-blue-600 hover:text-blue-800 hover:cursor-pointer"
    >
      ✕
    </button>
  </span>
</template>
