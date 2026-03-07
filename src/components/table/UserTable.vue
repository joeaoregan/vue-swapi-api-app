<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  planets: Array,
  users: Array,
  columns: Array,
  filterKey: String,
  ready: Boolean,
});

// sorting state
const sortKey = ref("");
const sortOrders = ref(props.columns.reduce((o, key) => ((o[key] = 1), o), {}));

function capitalFirstLetter(str) {
  // helper function to capitalise first letter of string
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function getPlanetName(url) {
  function isPlanet(planet) {
    return planet.url === url;
  }

  // if (planets.find(isPlanet)) {
  //   var p = planets.find(isPlanet);
  const p = props.planets.find(isPlanet);
  if (p) {
    return {
      name: p.name,
      diameter: p.diameter,
      climate: capitalFirstLetter(p.climate),
      population: p.population,
    };
  } else {
    return { name: "Loading" };
  }
}

function formatOutput(key, text) {
  // if (key === "created" || key == "edited") {
  //   var date = new Date(text);
  //   return new Intl.DateTimeFormat("en-IE", {
  //     dateStyle: "short",
  //     timeStyle: "medium",
  //   }).format(date);
  // } else
  if (key === "homeworld") {
    return getPlanetName(text);
  } else {
    return text;
  }
}

const filteredData = computed(() => {
  // let { users, filterKey } = props;
  // if (filterKey) {
  // filterKey = filterKey.toLowerCase();

  let users = props.users || [];

  if (props.filterKey) {
    const fk = props.filterKey.toLowerCase();
    users = users.filter((user) => 
      Object.keys(user).some((key) => 
        // String(user[key]).toLowerCase().indexOf(fk) > -1;
        String(user[key]).toLowerCase().includes(fk)
      )
    );
  }

  if (sortKey.value) {
    const key = sortKey.value;
    const order = sortOrders.value[key];
    users = users.slice().sort((a, b) => {
      const x = a[key];
      const y = b[key];
      return (x === y ? 0 : x > y ? 1 : -1) * order;
    });
  }

  return users;
});

function sortBy(key) {
  sortKey.value = key;
  sortOrders.value[key] *= -1;
}
</script>

<!-- <script>
export default {
  name: "UserTable",
  props: {},
  data() {
    return {
      planetDetails: "These are not the users you are looking for",
    };
  },
  methods: {},
};
</script> -->

<template>
  <div>
    <!-- <div v-if="ready">ready true</div>
    <div v-else>ready false</div> -->
    <div v-if="filteredData">
      <table v-if="filteredData.length" class="center">
        <thead>
          <tr>
            <th
              v-for="key in columns"
              :key="key"
              @click="sortBy(key)"
              :class="{ active: sortKey == key }"
              :title="
                'Sort By ' +
                capitalFirstLetter(key) +
                (sortOrders[key] > 0 ? ' Descending' : ' Ascending')
              "
            >
              {{ capitalFirstLetter(key) }}
              <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
              </span>
            </th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="entry in filteredData" :key="entry.name">
            <td v-for="key in columns" :key="key">
              <button
                v-if="
                  key === 'homeworld' &&
                  formatOutput(key, entry[key]).name != 'unknown' &&
                  formatOutput(key, entry[key]).name != 'Loading'
                "
                class="button"
                @click="$emit('togglePopup', formatOutput(key, entry[key]))"
                :title="
                  'Click For ' + formatOutput(key, entry[key]).name + ' Info'
                "
              >
                {{ formatOutput(key, entry[key]).name }}
              </button>

              <p
                v-else-if="
                  key === 'homeworld' &&
                  formatOutput(key, entry[key]).name === 'Loading'
                "
              >
                {{ capitalFirstLetter(formatOutput(key, entry[key]).name) }}
              </p>

              <p
                v-else-if="
                  key === 'homeworld' &&
                  formatOutput(key, entry[key]).name === 'unknown'
                "
              >
                {{ capitalFirstLetter(formatOutput(key, entry[key]).name) }}
              </p>
              <p v-else>
                {{ formatOutput(key, entry[key]) }}
              </p>
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
          <!-- <p>{{ planetDetails }}</p> -->
           <p>These are not the users you are looking for</p>
        </div>
        <p v-else id="users-loading">A Long Time Ago In A Galaxy Far Away...</p>
      </div>
    </div>
  </div>
</template>

<style>
@import "./style.css";
</style>
