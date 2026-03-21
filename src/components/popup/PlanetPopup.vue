<template>
  <div class="popup" @click.self="emit('toggle-planet')">
    <transition name="popup-fade">
      <div class="popup-inner">
        <h1>{{ planet.name }}</h1>

        <div v-if="planet.diameter" class="stats-container">
          <p>
            <strong>Diameter:</strong>
            {{ formatStringOrNumber(planet.diameter) }}
          </p>
          <p><strong>Climate:</strong> {{ planet.climate }}</p>
          <p>
            <strong>Population:</strong>
            {{ formatStringOrNumber(planet.population) }}
          </p>
        </div>

        <div v-else class="loader-wrapper">
          <div class="saber-spinner"></div>
          <p>Accessing Imperial Archives...</p>
        </div>

        <button class="button close-btn" @click="emit('toggle-planet')">
          Close
        </button>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from "vue";

const props = defineProps({
  planet: Object,
});

const emit = defineEmits(["toggle-planet"]);

function capitalFirstLetter(word) {
  return word ? word.charAt(0).toUpperCase() + word.slice(1) : "";
}

function formatStringOrNumber(value) {
  if (!value || value === "unknown") return "Unknown";
  const num = parseInt(value);
  return isNaN(num) ? capitalFirstLetter(value) : num.toLocaleString();
}

function handleKeydown(event) {
  if (event.key === "Escape") {
    emit("toggle-planet");
  }
}

onMounted(() => {
  window.addEventListener("keydown", handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener("keydown", handleKeydown);
});
</script>

<style src="./style.css"></style>
