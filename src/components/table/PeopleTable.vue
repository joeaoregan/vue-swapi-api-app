<template>
  <div class="people-table">
    <table class="center" v-if="filteredData.length">
      <thead>
        <tr>
          <th
            v-for="key in columns"
            :key="key"
            @click="sortBy(key)"
            :class="{ active: sortKey === key }"
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
            <!-- NAME POPUP -->
            <template v-if="key === 'name'">
              <button class="popup-link" @click="$emit('toggle-person', entry)">
                <span v-html="highlight(entry.name, filterKey)"></span>
              </button>
            </template>

            <!-- HOMEWORLD POPUP -->
            <template v-else-if="key === 'homeworld'">
              <button
                v-if="formatOutput(key, entry[key]).name !== 'unknown'"
                class="popup-link"
                @click="$emit('toggle-planet', formatOutput(key, entry[key]))"
              >
                {{ formatOutput(key, entry[key]).name }}
              </button>
              <span v-else>{{ formatOutput(key, entry[key]).name }}</span>
            </template>

            <!-- OTHER FIELDS -->
            <template v-else>
              <span
                v-html="highlight(formatOutput(key, entry[key]), filterKey)"
              ></span>
            </template>
          </td>
        </tr>
      </tbody>
    </table>

    <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      :pageSize="pageSize"
      @update:currentPage="currentPage = $event"
      @update:pageSize="pageSize = $event"
    />
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import Pagination from "@/components/pagination/Pagination.vue";

const props = defineProps({
  planets: Array,
  users: Array,
  columns: Array,
  filterKey: String,
});

const sortKey = ref("");
const sortOrders = ref(props.columns.reduce((o, k) => ((o[k] = 1), o), {}));

function capitalFirstLetter(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function getPlanetName(url) {
  const p = props.planets.find((x) => x.url === url);
  return p ? p : { name: "unknown" };
}

function formatOutput(key, value) {
  if (key === "created" || key === "edited") {
    return new Intl.DateTimeFormat("en-IE", {
      dateStyle: "short",
      timeStyle: "medium",
    }).format(new Date(value));
  }
  return key === "homeworld" ? getPlanetName(value) : value;
}

function highlight(text, query) {
  if (!query) return text;
  return String(text).replace(
    new RegExp(`(${query})`, "gi"),
    "<mark>$1</mark>",
  );
}

const filteredData = computed(() => {
  let data = props.users;
  if (props.filterKey) {
    const q = props.filterKey.toLowerCase();
    data = data.filter((u) =>
      Object.values(u).some((v) => String(v).toLowerCase().includes(q)),
    );
  }
  if (sortKey.value) {
    const key = sortKey.value;
    const order = sortOrders.value[key];
    data = [...data].sort((a, b) => (a[key] > b[key] ? 1 : -1) * order);
  }
  return data;
});

function sortBy(key) {
  sortKey.value = key;
  sortOrders.value[key] *= -1;
}

const currentPage = ref(1);
const pageSize = ref(10);

const totalPages = computed(() =>
  Math.ceil(filteredData.value.length / pageSize.value),
);

const paginatedData = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value;
  return filteredData.value.slice(start, start + pageSize.value);
});
</script>

<style src="./style.css"></style>
