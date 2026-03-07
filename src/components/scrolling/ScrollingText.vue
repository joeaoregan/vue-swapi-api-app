<template>
  <div class="aspect-169">
    <div class="fade-top"></div>

    <button class="restart-btn" @click="restartCrawl">Restart Crawl</button>

    <div class="wrapper">
      <div class="scroll-text" :key="animationKey">
        <h1>STAR WARS API</h1>

        <h2>Front-end developer assignment</h2>

        <p>
          The project retrieves character data from the Star Wars API,
          displaying essential details such as name, height, mass, creation
          date, and homeworld. Each planet is fetched dynamically, revealing
          climate, population, and diameter through a custom modal component.
        </p>

        <p>
          Table implemented containing a list of users with the following
          columns: name, height, mass, created, edited, planet name
        </p>

        <p>
          When the user clicks on the planet name link a popup is displayed
          showing the following information regarding the planet: name,
          diameter, climate, population
        </p>

        <p>
          Sorting and filtering are powered by computed properties, ensuring
          smooth and efficient interaction. A search feature allows users to
          quickly locate characters, while incorrect queries trigger a playful
          Jedi Mind Trick animation.
        </p>

        <p>
          The interface is fully responsive, adapting seamlessly from mobile to
          desktop. A light-mode toggle enhances readability, and accessibility
          considerations ensure a user-friendly experience for all.
        </p>

        <p>
          This project strengthened my understanding of component-based
          architecture, API integration, and UI/UX design. Special thanks to the
          creators of the Star Wars API, and to the developers whose tutorials
          inspired this work.
        </p>

        <p>May this forced API joke be with you</p>

        <p>End of transmission.</p>

        <p>Created by Joe O'Regan who could really do with a job.</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      animationKey: 0,
    };
  },
  methods: {
    restartCrawl() {
      this.animationKey++; // force element recreation → restart animation
    },
  },
};
</script>

<style src="./style.css"></style>
