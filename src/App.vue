<script setup>
import { RouterLink, RouterView } from "vue-router";
import Navbar from "./components/Navbar.vue";
import SideBar from "./components/SideBar.vue";
</script>

<template>
  <div class="coba w-100">
    <RouterView />
  </div>
</template>

<style>
.wrapper-icon {
  width: 42px;
  height: 42px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.icons {
  width: 18px;
  height: 18px;
}
.inputs {
  width: 268px;
  height: 42px;
}
.inputs input {
  padding: 12px 0 12px 12px;
}

/* toogle form */
.toogle-input {
  top: 110%;
  z-index: 10;
  background: white;
  border: 1px solid #eeeeee;
  padding: 10px 0;
  border-radius: 8px;
  box-shadow: 0px 4px 20px 0px #6a6a6a1a;
}
.list-unit:hover {
  background: #96c0ca;
  color: white;
  cursor: pointer;
}
</style>
