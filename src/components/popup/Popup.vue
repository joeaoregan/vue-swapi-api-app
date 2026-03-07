<template>
  <div class="popup" ref="overlay" @click="handleClickOutside">
    <div v-if="display" class="popup-inner">
      <h1>{{ name }}</h1>

      <p>
        <strong>Diameter:</strong> {{ formatStringOrNumber(diameter) }}<br />
        <strong>Climate:</strong> {{ climate }}<br />
        <strong>Population:</strong> {{ formatStringOrNumber(population) }}
      </p>

      <button
        class="popup-close button"
        @click="emit('togglePopup', 'close button pressed')"
      >
        Close
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const props = defineProps({
  name: String,
  diameter: String,
  climate: String,
  population: String,
});

const emit = defineEmits(["togglePopup"]);

const display = ref(true);
const overlay = ref(null);

function capitalFirstLetter(word) {
  if (!word) return "";
  return word.charAt(0).toUpperCase() + word.slice(1);
}

function formatStringOrNumber(value) {
  if (!value) return "";
  return parseInt(value)
    ? parseInt(value).toLocaleString()
    : capitalFirstLetter(value);
}

function handleKeydown(event) {
  if (event.key === "Escape") {
    emit("togglePopup", "escape pressed");
  }
}

function handleClickOutside(event) {
  // If the click target *is* the overlay, close the popup
  if (event.target === overlay.value) {
    emit("togglePopup", "clicked outside");
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
