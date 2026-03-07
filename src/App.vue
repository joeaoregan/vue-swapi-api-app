<template>
  <div>
    <Navbar></Navbar>
    <DarkModeToggle />
    <router-view />
  </div>
</template>

<script>
import Navbar from "./components/navbar/navbar.vue";
import DarkModeToggle from "./components/darkmode/DarkModeToggle.vue";

export default {
  components: {
    Navbar,
    DarkModeToggle,
  },
};
</script>
