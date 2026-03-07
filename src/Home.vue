<template>
  <div>
    <img :alt="imgAlt" src="./assets/logo.png" width="100" />

    <form v-show="people != null" id="search">
      Search <s>Your Feelings</s> Users:
      <input name="query" v-model="searchQuery" />
    </form>

    <LoadingScreen v-if="!ready" />

    <UserTable
      v-else
      @toggle-popup="togglePopup"
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
  </div>
</template>

<script setup>
import { ref } from "vue";
const searchQuery = ref("");
</script>

<script>
import PlanetPopup from "./components/popup/Popup"; // automatically registered component
import UserTable from "./components/table/UserTable.vue";
import LoadingScreen from "./components/loading/LoadingScreen.vue";
import axios from "axios";

export default {
  name: "App",

  components: {
    UserTable,
    LoadingScreen,
  },

  data() {
    return {
      ready: false,
      displayPopup: false,
      imgAlt: "Page logo image",
      people: [],
      planet: {},
      planets: [],
      gridColumns: ["name", "height", "mass", "created", "edited", "homeworld"],
    };
  },

  methods: {
    togglePopup(planet) {
      if (planet != null) {
        this.planet.name = planet.name;
        this.planet.diameter = planet.diameter;
        this.planet.climate = planet.climate;
        this.planet.population = planet.population;
      }
      this.displayPopup = !this.displayPopup;
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
