<template>
  <div>
    <img :alt="imgAlt" src="@/assets/logo.png" width="100" />

    <form v-show="people.length" id="search">
      Search <s>Your Feelings</s> Users:
      <SearchBox v-model="searchQuery" />
    </form>

    <div class="table-switcher" :class="activeTable" v-if="ready">
      <button
        type="button"
        :class="{ active: activeTable === 'people' }"
        @click="activeTable = 'people'"
      >
        People
      </button>

      <button
        type="button"
        :class="{ active: activeTable === 'planets' }"
        @click="activeTable = 'planets'"
      >
        Planets
      </button>
    </div>

    <LoadingScreen v-if="!ready" />

    <PeopleTable
      v-if="activeTable === 'people' && ready"
      @toggle-planet="togglePlanetPopup"
      @toggle-person="togglePersonPopup"
      :planets="planets"
      :users="people"
      :columns="gridColumns"
      :filter-key="searchQuery"
    />

    <PlanetsTable
      v-if="activeTable === 'planets' && ready"
      :planets="planets"
      @toggle-planet="togglePlanetPopup"
    />

    <PlanetPopup
      v-if="showPlanetPopup"
      :planet="planet"
      @toggle-planet="togglePlanetPopup"
    />

    <PersonPopup
      v-if="showPersonPopup"
      :person="selectedPerson"
      @toggle-person="togglePersonPopup"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

import SearchBox from "@/components/search/SearchBox.vue";
import PeopleTable from "@/components/table/PeopleTable.vue";
import PlanetsTable from "@/components/table/PlanetsTable.vue";
import LoadingScreen from "@/components/loading/LoadingScreen.vue";
import PlanetPopup from "@/components/popup/PlanetPopup.vue";
import PersonPopup from "@/components/popup/PersonPopup.vue";

const activeTable = ref("people");
const ready = ref(false);
const searchQuery = ref("");

const people = ref([]);
const planets = ref([]);

const showPlanetPopup = ref(false);
const showPersonPopup = ref(false);

const planet = ref({});
const selectedPerson = ref(null);

const imgAlt = "Page logo image";

const gridColumns = [
  "name",
  "height",
  "mass",
  "created",
  "edited",
  "homeworld",
];

function togglePlanetPopup(planetData) {
  if (planetData) {
    planet.value = { ...planetData };
  }
  showPlanetPopup.value = !showPlanetPopup.value;
}

function togglePersonPopup(personData) {
  if (personData) {
    const homeworldName =
      planets.value.find((p) => p.url === personData.homeworld)?.name ||
      "Unknown";

    selectedPerson.value = {
      ...personData,
      homeworldName,
    };
  }
  showPersonPopup.value = !showPersonPopup.value;
}

async function loadPeople() {
  let nextUrl = "https://swapi.dev/api/people/";
  while (nextUrl) {
    const response = await axios.get(nextUrl);
    people.value.push(...response.data.results);
    nextUrl = response.data.next;
  }
}

async function loadPlanets() {
  let nextUrl = "https://swapi.dev/api/planets/";
  while (nextUrl) {
    const response = await axios.get(nextUrl);
    planets.value.push(...response.data.results);
    nextUrl = response.data.next;
  }
}

onMounted(async () => {
  document.title = "Joe O'Regan SWAPI App";
  await Promise.all([loadPeople(), loadPlanets()]);
  ready.value = true;
});
</script>
