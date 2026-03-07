<template>
  <div class="popup" @click.self="emit('toggle-person')">
    <transition name="popup-fade">
      <div class="popup-inner">
        <h2>{{ person.name }}</h2>

        <p><strong>Height:</strong> {{ person.height }}</p>
        <p><strong>Mass:</strong> {{ person.mass }}</p>
        <p><strong>Birth Year:</strong> {{ person.birth_year }}</p>
        <p><strong>Gender:</strong> {{ person.gender }}</p>
        <p><strong>Homeworld:</strong> {{ person.homeworldName }}</p>

        <button class="button" @click="emit('toggle-person')">Close</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from "vue";

const props = defineProps({
  person: Object,
});

const emit = defineEmits(["toggle-person"]);

function handleKeydown(event) {
  if (event.key === "Escape") {
    emit("toggle-person");
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
