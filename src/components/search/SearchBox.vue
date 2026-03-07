<template>
  <div class="search-wrapper">
    <!-- <span class="search-icon">🔍</span> -->
    <span class="search-icon">🔍︎</span>

    <input
      :value="modelValue"
      @input="updateValue($event.target.value)"
      placeholder="Search users..."
    />

    <button
      v-if="modelValue"
      class="clear-btn"
      @click.prevent="clear"
      aria-label="Clear search"
    >
      ✕
    </button>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from "vue";

const props = defineProps({
  modelValue: String,
});

const emit = defineEmits(["update:modelValue"]);

function updateValue(val) {
  emit("update:modelValue", val);
}

function clear() {
  emit("update:modelValue", "");
}

function handleKeydown(event) {
  if (event.key === "Escape") {
    emit("update:modelValue", "");
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
