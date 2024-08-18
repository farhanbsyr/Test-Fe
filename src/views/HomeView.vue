<script setup>
import Title from "@/components/Title.vue";
import RoomForm from "@/components/RoomForm.vue";
import { ref } from "vue";
import SideBar from "@/components/SideBar.vue";
import Navbar from "@/components/Navbar.vue";
const isForm = ref(false);
const handleIsFormUpdate = (msg) => {
  isForm.value = msg;
};
</script>

<template>
  <div>
    <Navbar />
    <main class="h-100 tes d-flex justify-content-start">
      <div class="sidebar">
        <SideBar />
      </div>
      <div class="wrapper-meeting w-100">
        <!-- title -->
        <Title @update:isForm="handleIsFormUpdate" class="wrapper-title" />
        <!-- end Title -->
        <room-form v-if="isForm" />
      </div>
    </main>
  </div>
</template>

<style scoped>
.wrapper-meeting {
  padding: 24px 28px 36px 0px;
}
.wrapper-title {
  margin-bottom: 32px;
}
</style>
