<template>
  <nav class="navbar">
    <div class="icon">
      <router-link to="/">
        <img alt="Home" src="@/assets/logo2.png" width="30" />
      </router-link>
    </div>

    <div class="icon">
      <router-link to="/about">
        <img alt="About" src="@/assets/info.png" width="30" />
      </router-link>
    </div>

    <div class="icon">
      <a
        href="https://www.linkedin.com/in/joeaoregan/"
        target="_blank"
        rel="noopener noreferrer"
        title="Joe O'Regan - LinkedIn"
      >
        <img alt="LinkedIn" src="@/assets/linkedin_logo.png" width="30" />
      </a>
    </div>

    <div class="icon">
      <a
        href="https://github.com/joeaoregan/"
        target="_blank"
        rel="noopener noreferrer"
        title="Joe O'Regan - GitHub"
      >
        <img alt="GitHub" src="@/assets/github_logo.png" width="30" />
      </a>
    </div>

    <div class="icon">
      <a
        href="https://github.com/joeaoregan/vue-front-end-dev-app"
        target="_blank"
        rel="noopener noreferrer"
        title="Repo"
      >
        <img alt="Repo" src="@/assets/repo.png" width="30" />
      </a>
    </div>
  </nav>
</template>

<script setup>
// No reactive state needed — everything is static
</script>

<style src="./style.css"></style>
