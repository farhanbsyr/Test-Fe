<template>
  <div class="d-flex justify-content-between align-items-center">
    <div class="left d-flex wrapper-title justify-content-start gap-3">
      <button
        v-if="isForm"
        @click="handleHome"
        class="wrapper-icon border-0 bg-primary"
      >
        <img src="../assets/images/left-arrow.svg" alt="arrow-back" />
      </button>
      <div class="title">
        <h1 class="fs-3 fw-bolder m-0 mb-1">Ruang Meeting</h1>
        <nav>
          <ol class="breadcrumb mb-2">
            <a
              class="home-link breadcrumb-item font-size-large active text-decoration-none"
              aria-current="page"
              >Ruang Meeting</a
            >
            <div
              v-if="isForm"
              class="icons p-0 mx-1 d-flex justify-content-center align-items-center"
            >
              <img src="../assets/images/right-arrow.svg" alt="right arrow" />
            </div>
            <a
              v-if="isForm"
              class="breadcrumb-item font-size-large text-decoration-none"
              >Pesan Ruangan</a
            >
          </ol>
        </nav>
      </div>
    </div>

    <div v-if="!isForm" class="button" @click="handleForm">
      <button class="btn btn-primary d-flex gap-2 align-items-center">
        <img src="../assets/images/plus.svg" alt="Plus" />
        <span class="fw-semibold me-1 text-white">Pesan Ruangan</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["update:isForm"]);
const isForm = ref(false);
const handleForm = () => {
  isForm.value = true;
  emit("update:isForm", isForm.value);
};
const handleHome = () => {
  isForm.value = false;
  emit("update:isForm", isForm.value);
};
</script>

<style></style>
