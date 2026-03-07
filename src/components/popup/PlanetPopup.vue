<template>
  <div class="popup" @click.self="$emit('toggle-planet')">
    <transition name="popup-fade">
      <div class="popup-inner">
        <h1>{{ planet.name }}</h1>

        <p>
          <strong>Diameter:</strong> {{ formatStringOrNumber(planet.diameter) }}<br />
          <strong>Climate:</strong> {{ planet.climate }}<br />
          <strong>Population:</strong> {{ formatStringOrNumber(planet.population) }}
        </p>

        <button class="button" @click="$emit('toggle-planet')">Close</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
const props = defineProps({
  planet: Object,
});

function capitalFirstLetter(word) {
  return word ? word.charAt(0).toUpperCase() + word.slice(1) : "";
}

function formatStringOrNumber(value) {
  return parseInt(value)
    ? parseInt(value).toLocaleString()
    : capitalFirstLetter(value);
}
</script>

<style src="./style.css"></style>
