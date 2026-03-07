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
      @toggle-planet="togglePlanetPopup"
      @toggle-person="togglePersonPopup"
      :planets="planets"
      :users="people"
      :columns="gridColumns"
      :filter-key="searchQuery"
      :ready="ready"
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
      showPlanetPopup: false,
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
    togglePlanetPopup(planet) {
      if (planet) {
        this.planet = { ...planet };
      }
      this.showPlanetPopup = !this.showPlanetPopup;
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
