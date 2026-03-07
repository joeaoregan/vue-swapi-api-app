<template>
  <button v-show="visible" class="scroll-to-top" @click="scrollToTop">
    ↑ Top
  </button>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const visible = ref(false);

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: "smooth" });
}

function handleScroll() {
  visible.value = window.scrollY > 200;
}

onMounted(() => window.addEventListener("scroll", handleScroll));
onUnmounted(() => window.removeEventListener("scroll", handleScroll));
</script>

<style scoped src="./style.css"></style>
