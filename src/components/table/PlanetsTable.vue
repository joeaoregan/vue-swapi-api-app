<template>
  <table class="sw-table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Climate</th>
        <th>Population</th>
        <th>Diameter</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="planet in planets" :key="planet.name">
        <td>{{ planet.name }}</td>
        <td>{{ planet.climate }}</td>
        <td>{{ planet.population }}</td>
        <td>{{ planet.diameter }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
defineProps({
  planets: {
    type: Array,
    required: true,
  },
});
</script>

<style>
.sw-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.sw-table th,
.sw-table td {
  border: 1px solid #444;
  padding: 8px;
  text-align: left;
  color: #ffc909;
}

.sw-table th {
  background: #222;
  font-weight: bold;
}
</style>
