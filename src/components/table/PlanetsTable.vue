<template>
  <div class="planets-table">
    <table class="center">
      <thead>
        <tr>
          <th
            v-for="col in columns"
            :key="col"
            @click="sortBy(col)"
            :class="{ active: sortKey === col }"
          >
            {{ capital(col) }}
            <span class="arrow" :class="sortOrders[col] > 0 ? 'asc' : 'dsc'"></span>
          </th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="planet in paginatedPlanets" :key="planet.name">
          <td>
            <button class="popup-link" @click="$emit('toggle-planet', planet)">
              {{ planet.name }}
            </button>
          </td>
          <td>{{ planet.climate }}</td>
          <td>{{ planet.population }}</td>
          <td>{{ planet.diameter }}</td>
        </tr>
      </tbody>
    </table>

    <div class="pagination">
      <button :disabled="currentPage === 1" @click="currentPage--">Prev</button>
      <span>Page {{ currentPage }} / {{ totalPages }}</span>
      <button :disabled="currentPage === totalPages" @click="currentPage++">Next</button>

      <select v-model="pageSize">
        <option :value="5">5</option>
        <option :value="10">10</option>
        <option :value="20">20</option>
      </select>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const props = defineProps({
  planets: Array,
});

const columns = ["name", "climate", "population", "diameter"];

const sortKey = ref("");
const sortOrders = ref(columns.reduce((o, k) => ((o[k] = 1), o), {}));

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
    data.sort((a, b) => (a[key] > b[key] ? 1 : -1) * order);
  }
  return data;
});

const currentPage = ref(1);
const pageSize = ref(10);

const totalPages = computed(() =>
  Math.ceil(sortedPlanets.value.length / pageSize.value),
);

const paginatedPlanets = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value;
  return sortedPlanets.value.slice(start, start + pageSize.value);
});

watch(pageSize, () => (currentPage.value = 1));
</script>

<style src="./style.css"></style>
