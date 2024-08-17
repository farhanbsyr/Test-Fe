<template>
  <div
    @mouseleave="hideModal"
    class="bg-white sidebar d-flex flex-column justify-content-start position-relative"
  >
    <router-link
      to="/dashboard"
      class="wrapper-icon home-place cursor-pointer"
      @mouseover="showModal"
    >
      <img src="../assets/images/home.svg" alt="dashboard" />
    </router-link>
    <div class="wrapper-icon file-place bg-primary">
      <img src="../assets/images/file.svg" alt="room meeting" />
    </div>
    <side-modal v-if="modal" />
  </div>
</template>

<script setup>
import { RouterLink } from "vue-router";
import SideModal from "./SideModal.vue";
import { ref } from "vue";
const modal = ref(false);

const emit = defineEmits("");

const showModal = () => {
  modal.value = true;
};
const hideModal = () => {
  modal.value = false;
};
</script>

<style scoped>
.sidebar {
  padding: 24px 15px;
  width: 72px;
  box-shadow: 4px 0px 20px 0px rgba(106, 106, 106, 0.1);
  gap: 12px;
  transition: width 0.3s ease;
}

.wrapper-icon {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  border-radius: 5px;
  transition: background-color 0.3s ease; /* Transisi warna background saat hover */
}

.home-place img,
.file-place img {
  width: 24px; /* Sesuaikan ukuran ikon */
  height: 24px; /* Sesuaikan ukuran ikon */
}
</style>
