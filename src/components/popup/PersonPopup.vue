<template>
  <div class="popup" @click.self="$emit('toggle-person')">
    <transition name="popup-fade">
      <div class="popup-inner">
        <h2>{{ person.name }}</h2>

        <ul>
          <li><strong>Height:</strong> {{ person.height }}</li>
          <li><strong>Mass:</strong> {{ person.mass }}</li>
          <li><strong>Birth Year:</strong> {{ person.birth_year }}</li>
          <li><strong>Gender:</strong> {{ person.gender }}</li>
          <li><strong>Homeworld:</strong> {{ person.homeworldName }}</li>
        </ul>

        <button class="button" @click="$emit('toggle-person')">Close</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
const props = defineProps({
  person: Object,
});
</script>

<style src="./style.css"></style>
