<template>
  <div>
    <!-- TABLE -->
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
        <tr v-for="planet in paginatedPlanets" :key="planet.name">
          <td>
            <button
              class="button"
              @click="
                $emit('toggle-planet', {
                  name: planet.name,
                  climate: planet.climate,
                  population: planet.population,
                  diameter: planet.diameter,
                })
              "
              :title="`View details for ${planet.name}`"
            >
              {{ planet.name }}
            </button>
          </td>

          <td>{{ planet.climate }}</td>
          <td>{{ planet.population }}</td>
          <td>{{ planet.diameter }}</td>
        </tr>
      </tbody>
    </table>

    <!-- PAGINATION -->
    <div class="pagination">
      <button :disabled="currentPage === 1" @click="currentPage--">Prev</button>

      <span>Page {{ currentPage }} / {{ totalPages }}</span>

      <button :disabled="currentPage === totalPages" @click="currentPage++">
        Next
      </button>

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

const emit = defineEmits(["toggle-planet"]);

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
  currentPage.value = 1; // reset page on sort
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

// PAGINATION
const currentPage = ref(1);
const pageSize = ref(10);

const totalPages = computed(() =>
  Math.ceil(sortedPlanets.value.length / pageSize.value),
);

const paginatedPlanets = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value;
  return sortedPlanets.value.slice(start, start + pageSize.value);
});

// reset page when page size changes
watch(pageSize, () => {
  currentPage.value = 1;
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

/* PAGINATION */
.pagination {
  margin-top: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
}

.pagination button {
  padding: 6px 12px;
  background: #ffc909;
  border: none;
  cursor: pointer;
  border-radius: 4px;
}

.pagination button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.pagination select {
  padding: 4px 8px;
  background: #222;
  color: #ffc909;
  border: 1px solid #444;
  border-radius: 4px;
}

.button {
  background: none;
  border: none;
  color: #ffc909;
  cursor: pointer;
  font-weight: bold;
  text-decoration: underline;
}

.button:hover {
  color: #ffea61;
}
</style>
