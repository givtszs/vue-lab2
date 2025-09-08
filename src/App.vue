<script setup>
import Demo from "./components/Demo.vue";
</script>

<template>
  <div class="flex justify-center gap-8 mb-8">
    <a href="https://vite.dev" target="_blank">
      <img
        src="/vite.svg"
        alt="Vite logo"
        class="h-28 p-6 transition hover:drop-shadow-[0_0_2em_#646cffaa]"
      />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img
        src="./assets/vue.svg"
        alt="Vue logo"
        class="h-28 p-6 transition hover:drop-shadow-[0_0_2em_#42b883aa]"
      />
    </a>
  </div>

  <Demo msg="Vite + Vue" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
