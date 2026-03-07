<template>
  <div id="app">
    <Navbar />
    <DarkModeToggle />
    <router-view />
  </div>
</template>

<script setup>
import Navbar from "./components/navbar/navbar.vue";
import DarkModeToggle from "./components/darkmode/DarkModeToggle.vue";
</script>

<style>
/* global styles if you had any */
</style>
