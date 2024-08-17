<template>
  <!-- Modal -->
  <div
    class="side-modal bg-white border-1 border-black h-100 position-absolute"
  >
    <router-link
      to="/dashboard"
      class="d-flex cursor-pointer wrapper-sec align-items-center gap-2 section-ds bg-primary rounded-end-3"
    >
      <h1 class="font-size-larger text-white fw-bold m-0">Dashboard</h1>
    </router-link>
  </div>
</template>

<script setup>
import { RouterLink } from "vue-router";
</script>

<style scoped>
.side-modal {
  top: 0;
  left: 65px;
  width: 200px;
  height: 500px;
  z-index: 1000;
  background: white;
  padding: 24px 0px;
  box-shadow: 8px 0px 20px 0px rgba(106, 106, 106, 0.1);
}
.wrapper-sec {
  height: 42px;
  padding-left: 12px;
  margin-right: 20px;
  cursor: pointer;
}
</style>
