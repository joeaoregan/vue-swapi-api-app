<template>
  <table class="sw-table">
    <thead>
      <tr>
        <th
          v-for="col in columns"
          :key="col"
          @click="sortBy(col)"
          :class="{ active: sortKey === col }"
        >
          {{ capital(col) }}
          <span
            class="arrow"
            :class="sortOrders[col] > 0 ? 'asc' : 'dsc'"
          ></span>
        </th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="planet in sortedPlanets" :key="planet.name">
        <td>{{ planet.name }}</td>
        <td>{{ planet.climate }}</td>
        <td>{{ planet.population }}</td>
        <td>{{ planet.diameter }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  planets: Array,
});

// columns for sorting
const columns = ["name", "climate", "population", "diameter"];

// sorting state
const sortKey = ref("");
const sortOrders = ref(columns.reduce((o, key) => ((o[key] = 1), o), {}));

function capital(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function sortBy(key) {
  sortKey.value = key;
  sortOrders.value[key] *= -1;
}

const sortedPlanets = computed(() => {
  let data = [...props.planets];

  if (sortKey.value) {
    const key = sortKey.value;
    const order = sortOrders.value[key];

    data.sort((a, b) => {
      const x = a[key];
      const y = b[key];
      return (x === y ? 0 : x > y ? 1 : -1) * order;
    });
  }

  return data;
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
  cursor: pointer;
}

.sw-table th.active {
  background: #333;
}

.arrow {
  margin-left: 6px;
  border: solid #ffc909;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 3px;
}

.arrow.asc {
  transform: rotate(-135deg);
}

.arrow.dsc {
  transform: rotate(45deg);
}
</style>
