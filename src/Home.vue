<template>
  <div>
    <img :alt="imgAlt" src="./assets/logo.png" width="100" />

    <form v-show="people != null" id="search">
      Search <s>Your Feelings</s> Users:
      <SearchBox v-model="searchQuery" />
    </form>

    <LoadingScreen v-if="!ready" />

    <UserTable
      v-else
      @toggle-popup="togglePopup"
      @togglePersonPopup="togglePersonPopup"
      :planets="planets"
      :users="people"
      :columns="gridColumns"
      :filter-key="searchQuery"
      :ready="ready"
    />

    <PlanetPopup
      v-if="displayPopup"
      @toggle-popup="togglePopup"
      :name="planet.name"
      :diameter="planet.diameter"
      :climate="planet.climate"
      :population="planet.population"
    />

    <PersonPopup
      v-if="showPersonPopup"
      :person="selectedPerson"
      @toggle-popup="togglePersonPopup"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import SearchBox from "@/components/search/SearchBox.vue";

const searchQuery = ref("");
</script>

<script>
import PlanetPopup from "./components/popup/PlanetPopup.vue";
import PersonPopup from "./components/popup/PersonPopup.vue";
import UserTable from "./components/table/UserTable.vue";
import LoadingScreen from "./components/loading/LoadingScreen.vue";
import axios from "axios";

export default {
  name: "App",

  components: {
    UserTable,
    LoadingScreen,
    PlanetPopup,
    PersonPopup,
  },

  data() {
    return {
      ready: false,
      displayPopup: false,
      showPersonPopup: false,
      imgAlt: "Page logo image",
      people: [],
      planet: {},
      planets: [],
      selectedPerson: null,
      gridColumns: ["name", "height", "mass", "created", "edited", "homeworld"],
    };
  },

  methods: {
    togglePopup(planet) {
      if (planet) {
        this.planet = {
          name: planet.name,
          diameter: planet.diameter,
          climate: planet.climate,
          population: planet.population,
        };
      }
      this.displayPopup = !this.displayPopup;
    },

    togglePersonPopup(person) {
      if (person) {
        const homeworldName =
          this.planets.find((p) => p.url === person.homeworld)?.name ||
          "Unknown";

        this.selectedPerson = {
          ...person,
          homeworldName,
        };
      }
      this.showPersonPopup = !this.showPersonPopup;
    },

    async loadPeople() {
      let nextUrl = "https://swapi.dev/api/people/";

      while (nextUrl) {
        const response = await axios.get(nextUrl);
        this.people.push(...response.data.results);
        nextUrl = response.data.next;
        console.log("People page loaded");
      }

      console.log("All users loaded");
    },

    async loadPlanets() {
      let nextUrl = "https://swapi.dev/api/planets/";

      while (nextUrl) {
        const response = await axios.get(nextUrl);
        this.planets.push(...response.data.results);
        nextUrl = response.data.next;
        console.log("Planets page loaded");
      }

      console.log("All planets loaded");
    },
  },

  async mounted() {
    document.title = "Joe O'Regan SWAPI App";

    await Promise.all([this.loadPeople(), this.loadPlanets()]);

    this.ready = true;
    console.log("Ready:", this.ready);
  },
};
</script>
