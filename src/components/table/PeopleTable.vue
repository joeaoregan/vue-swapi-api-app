<script setup>
import { ref, computed, watch } from "vue";
import Pagination from "@/components/pagination/Pagination.vue";
import ScrollToTop from "@/components/scroll/ScrollToTop.vue";

const props = defineProps({
  planets: Array,
  users: Array,
  columns: Array,
  filterKey: String,
  ready: Boolean,
});

// sorting
const sortKey = ref("");
const sortOrders = ref(props.columns.reduce((o, key) => ((o[key] = 1), o), {}));

// pagination
const currentPage = ref(1);
const pageSize = ref(10);

function capitalFirstLetter(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function getPlanetName(url) {
  const planet = props.planets.find((p) => p.url === url);

  if (planet) {
    return {
      name: planet.name,
      diameter: planet.diameter,
      climate: capitalFirstLetter(planet.climate),
      population: planet.population,
    };
  }
  return { name: "Loading" };
}

function formatOutput(key, value) {
  if (key === "created" || key === "edited") {
    const date = new Date(value);
    return new Intl.DateTimeFormat("en-IE", {
      dateStyle: "short",
      timeStyle: "medium",
    }).format(date);
  }
  return key === "homeworld" ? getPlanetName(value) : value;
}

// search highlight
function highlight(text, query) {
  if (!query || !text) return text;

  const safe = String(text);
  const regex = new RegExp(`(${query})`, "gi");

  return safe.replace(regex, `<mark>$1</mark>`);
}

const filteredData = computed(() => {
  let userData = props.users || [];

  if (props.filterKey) {
    const fk = props.filterKey.toLowerCase();
    userData = userData.filter((user) =>
      Object.keys(user).some((key) =>
        String(user[key]).toLowerCase().includes(fk),
      ),
    );
  }

  if (sortKey.value) {
    const key = sortKey.value;
    const order = sortOrders.value[key];
    userData = userData.slice().sort((a, b) => {
      const x = a[key];
      const y = b[key];
      return (x === y ? 0 : x > y ? 1 : -1) * order;
    });
  }

  return userData;
});

function sortBy(key) {
  sortKey.value = key;
  sortOrders.value[key] *= -1;
}

// pagination logic
const totalPages = computed(() =>
  Math.ceil(filteredData.value.length / pageSize.value),
);

const paginatedData = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value;
  return filteredData.value.slice(start, start + pageSize.value);
});

// reset page when filtering
watch(
  () => props.filterKey,
  () => {
    currentPage.value = 1;
  },
);

watch(pageSize, () => {
  currentPage.value = 1;
});
</script>

<template>
  <div>
    <div v-if="filteredData">
      <table v-if="filteredData.length" class="center">
        <thead>
          <tr>
            <th
              v-for="key in columns"
              :key="key"
              @click="sortBy(key)"
              :class="{ active: sortKey == key }"
              :title="`Sort By ${capitalFirstLetter(key)} ${
                sortOrders[key] > 0 ? 'Descending' : 'Ascending'
              }`"
            >
              {{ capitalFirstLetter(key) }}
              <span
                class="arrow"
                :class="sortOrders[key] > 0 ? 'asc' : 'dsc'"
              ></span>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="entry in paginatedData" :key="entry.name">
            <td v-for="key in columns" :key="key">
              <!-- NAME → person popup -->
              <template v-if="key === 'name'">
                <button
                  class="button"
                  @click="$emit('toggle-person', entry)"
                  :title="`View details for ${entry.name}`"
                >
                  <span v-html="highlight(entry.name, filterKey)"></span>
                </button>
              </template>

              <!-- HOMEWORLD → planet popup -->
              <template v-else-if="key === 'homeworld'">
                <button
                  v-if="
                    formatOutput(key, entry[key]).name !== 'unknown' &&
                    formatOutput(key, entry[key]).name !== 'Loading'
                  "
                  class="button"
                  @click="$emit('toggle-planet', formatOutput(key, entry[key]))"
                  :title="`Click For ${
                    formatOutput(key, entry[key]).name
                  } Info`"
                >
                  {{ formatOutput(key, entry[key]).name }}
                </button>

                <p v-else>
                  {{ capitalFirstLetter(formatOutput(key, entry[key]).name) }}
                </p>
              </template>

              <!-- OTHER COLUMNS -->
              <template v-else>
                <span
                  v-html="highlight(formatOutput(key, entry[key]), filterKey)"
                ></span>
              </template>
            </td>
          </tr>
        </tbody>
      </table>

      <div v-else>
        <div v-if="ready" id="user-not-found">
          <img
            alt="Jedi Mind Trick Image"
            src="../../assets/mind-trick.gif"
            width="400"
          />
          <p>These are not the users you are looking for</p>
        </div>
        <div v-else class="fade-in"></div>
      </div>

      <Pagination
        :currentPage="currentPage"
        :totalPages="totalPages"
        :pageSize="pageSize"
        @update:currentPage="currentPage = $event"
        @update:pageSize="pageSize = $event"
      />

      <ScrollToTop />
    </div>
  </div>
</template>

<style scoped src="./style.css"></style>
