<script setup>
const props = defineProps({
    isOpen: {
        type: Boolean,
        required: true,
        default: false,
    },
});
</script>
<template>
    <div
      v-if="props.isOpen"
      class="absolute left-0 right-0 mt-1 bg-white rounded-lg shadow-sm z-10"
    >
        <slot></slot>
    </div>
</template>
