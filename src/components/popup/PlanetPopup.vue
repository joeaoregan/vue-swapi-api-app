<template>
  <div class="popup" @click.self="$emit('toggle-popup')">
    <transition name="popup-fade">
      <div class="popup-inner">
        <h1>{{ name }}</h1>

        <p>
          <strong>Diameter:</strong> {{ formatStringOrNumber(diameter) }}<br />
          <strong>Climate:</strong> {{ climate }}<br />
          <strong>Population:</strong> {{ formatStringOrNumber(population) }}
        </p>

        <button class="button" @click="$emit('toggle-popup')">Close</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
const props = defineProps({
  name: String,
  diameter: String,
  climate: String,
  population: String,
});

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
</script>

<style src="./style.css"></style>
