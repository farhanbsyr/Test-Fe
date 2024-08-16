<template>
  <div class="bg-white sidebar d-flex flex-column justify-content-start">
    <router-link to="/dashboard" class="wrapper-icon home-place">
      <img src="../assets/images/home.svg" alt="dashboard" />
    </router-link>
    <div class="wrapper-icon file-place bg-primary">
      <img src="../assets/images/file.svg" alt="room meeting" />
    </div>
  </div>
</template>

<script setup>
import { RouterLink } from "vue-router";
</script>

<style scoped>
.sidebar {
  padding: 24px 15px;
  width: 72px;
  box-shadow: 4px 0px 20px 0px rgba(106, 106, 106, 0.1);
  gap: 12px;
}
</style>
