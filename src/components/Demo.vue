<script setup>
import { ref } from "vue";
import Button from "@/components/Button.vue";
import SearchableDropdown from "@/components/Dropdown/SearchableDropdown.vue";

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template>
  <div id="app" class="max-w-[1280px] mx-auto p-8 text-center">
    <h1 class="text-[3.2em] leading-tight font-bold">{{ msg }}</h1>

    <div class="p-8 flex flex-col items-center gap-10">
      <div class="flex items-center gap-3">
        <Button
          :label="`count is ${count}`"
          color="green"
          size="lg"
          @click="count++"
        >
          <template #icon>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-calculator-icon lucide-calculator"
            >
              <rect width="16" height="20" x="4" y="2" rx="2" />
              <line x1="8" x2="16" y1="6" y2="6" />
              <line x1="16" x2="16" y1="14" y2="18" />
              <path d="M16 10h.01" />
              <path d="M12 10h.01" />
              <path d="M8 10h.01" />
              <path d="M12 14h.01" />
              <path d="M8 14h.01" />
              <path d="M12 18h.01" />
              <path d="M8 18h.01" />
            </svg>
          </template>
        </Button>

        <Button label="Sky" color="blue" size="sm" />

        <Button
          label="Danger"
          color="red"
          size="md"
          @click="console.log('Danger detected!')"
        />
      </div>

      <div class="flex items-center gap-3">
        <SearchableDropdown
          :items="[
            { label: 'Item 1', value: 'item-1' },
            { label: 'Item 2', value: 'item-2' },
            { label: 'Item 3', value: 'item-3' },
          ]"
          :multiselect="true"
          @update:modelValue="console.log($event)"
        />
        <SearchableDropdown
          :items="[
            { label: 'Item 1', value: 'item-1' },
            { label: 'Item 2', value: 'item-2' },
            { label: 'Item 3', value: 'item-3' },
          ]"
          :multiselect="false"
          @update:modelValue="console.log($event)"
        />
      </div>
    </div>
  </div>
</template>
