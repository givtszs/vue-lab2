<script setup>
import Button from "@/components/Button.vue";
import SearchableDropdown from "@/components/Dropdown/SearchableDropdown.vue";
import { ref } from "vue";

defineProps({
  msg: String,
});

// 1st button
const count = ref(0);

// 2nd button
const buttonIcon = ref("fa-solid fa-dog");

const faIconIconsPool = [
  "fa-solid fa-home",
  "fa-solid fa-user",
  "fa-solid fa-search",
  "fa-solid fa-plus",
  "fa-solid fa-minus",
  "fa-solid fa-arrow-up",
  "fa-solid fa-arrow-down",
  "fa-solid fa-arrow-left",
  "fa-solid fa-arrow-right",
  "fa-solid fa-check",
  "fa-solid fa-times",
  "fa-solid fa-info-circle",
  "fa-solid fa-exclamation-circle",
  "fa-solid fa-question-circle",
  "fa-solid fa-lock",
  "fa-solid fa-unlock",
  "fa-solid fa-key",
];

const swapIcon = () => {
  buttonIcon.value = faIconIconsPool[Math.floor(Math.random() * faIconIconsPool.length)];
};

// 3rd button
const dangerTextSizePx = ref(0);
</script>

<template>
  <div id="app" class="max-w-[1280px] mx-auto p-8 text-center">
    <h1 class="text-[3.2em] leading-tight font-bold">{{ msg }}</h1>

    <div class="relative p-8 flex flex-col items-center gap-10">
      <div class="flex items-center gap-3">
        <Button
          :label="`count is ${count}`"
          color="green"
          size="lg"
          @click="count++"
        />

        <Button
          label="Sky"
          color="blue"
          size="sm"
          :icon="buttonIcon"
          @click="swapIcon"
        />

        <Button
          class="z-10"
          label="Danger!"
          color="red"
          size="md"
          @click="dangerTextSizePx += 10"
        />

        <span
          v-if="dangerTextSizePx > 0"
          class="absolute -bottom-10 inset-x-0 px-2 py-1 w-auto h-auto text-white bg-red-500 rounded z-1"
          :style="{ fontSize: `${dangerTextSizePx}px` }"
        >
          Danger !!!
        </span>
      </div>

      <div class="flex items-center gap-3">
        <div class="space-y-2">
          <div>Multi select</div>
          <SearchableDropdown
            :items="[
              { label: 'Item 1', value: 'item-1' },
              { label: 'Item 2', value: 'item-2' },
              { label: 'Item 3', value: 'item-3' },
            ]"
            :multiselect="true"
            @update:modelValue="console.log($event)"
          />
        </div>
        <div class="space-y-2">
          <div>Single select</div>
          <SearchableDropdown
            :items="[
              { label: 'Ten', value: 10 },
              { label: 'Eleven', value: 11 },
              { label: 'Twelve', value: 12 },
            ]"
            custom-item-class="bg-gray-100! outline-red-500! hover:bg-blue-100!"
            @update:modelValue="console.log($event)"
          />
        </div>
      </div>
    </div>
  </div>
</template>
