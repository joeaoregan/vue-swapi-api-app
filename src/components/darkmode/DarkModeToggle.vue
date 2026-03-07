<template>
  <button class="dark-toggle" @click="toggleDarkMode">
    {{ isDark ? "Light Mode" : "Dark Mode" }}
  </button>
</template>

<script setup>
import { ref, onMounted } from "vue";

const isDark = ref(false);

onMounted(() => {
  // Load saved preference
  isDark.value = localStorage.getItem("darkMode") === "true";
  document.body.classList.toggle("dark-mode", isDark.value);
});

function toggleDarkMode() {
  isDark.value = !isDark.value;
  document.body.classList.toggle("dark-mode", isDark.value);
  localStorage.setItem("darkMode", isDark.value);
}
</script>

<style scoped src="./style.css"></style>
