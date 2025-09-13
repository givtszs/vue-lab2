<script setup>
const props = defineProps({
  label: {
    type: String,
    default: "",
    required: true,
  },
});
</script>
<template>
  <li
    class="flex items-center ps-2 rounded-sm hover:bg-gray-100 hover:cursor-pointer"
  >
    <span class="w-full py-2 ms-2 text-sm font-medium text-gray-900 rounded-sm">
      {{ props.label }}
    </span>
  </li>
</template>
