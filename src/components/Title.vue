<template>
  <div class="d-flex wrapper-title justify-content-start gap-3">
    <div class="wrapper-icon bg-primary">
      <img src="../assets/images/left-arrow.svg" alt="arrow-back" />
    </div>
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
            class="icons p-0 mx-1 d-flex justify-content-center align-items-center"
          >
            <img src="../assets/images/right-arrow.svg" alt="right arrow" />
          </div>
          <router-link
            to="/form-room"
            class="breadcrumb-item font-size-large text-decoration-none"
            >Pesan Ruangan</router-link
          >
        </ol>
      </nav>
    </div>
  </div>
</template>

<script setup></script>

<style></style>
