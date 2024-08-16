<script setup>
import SideBar from "@/components/SideBar.vue";
import { RouterLink } from "vue-router";
import Navbar from "@/components/Navbar.vue";
</script>

<template>
  <div>
    <Navbar />
    <main class="d-flex">
      <div class="sidebar">
        <SideBar />
      </div>
      <div class="wrapper-meeting w-100">
        <!-- title -->
        <div
          class="container-fluid px-0 d-flex wrapper-title justify-content-between align-items-center"
        >
          <div class="title">
            <h1 class="fs-3 fw-bolder m-0 mb-1">Ruang Meeting</h1>
            <nav>
              <ol class="breadcrumb mb-2">
                <router-link
                  to="/form-room"
                  class="breadcrumb-item font-size-large text-decoration-none"
                  >Ruang Meeting</router-link
                >
              </ol>
            </nav>
          </div>
          <div class="button">
            <router-link
              to="/form-room"
              class="btn btn-primary d-flex gap-2 align-items-center"
            >
              <img src="../assets/images/plus.svg" alt="Plus" />
              <span class="fw-semibold me-1 text-white">Pesan Ruangan</span>
            </router-link>
          </div>
        </div>
        <!-- end Title -->
      </div>
    </main>
  </div>
</template>

<style scoped>
.wrapper-meeting {
  padding: 24px 32px 36px 28px;
}
.wrapper-title {
  margin-bottom: 32px;
}
.wrapper-room {
  padding: 20px 28px;
  border-color: #eeeeee;
  box-shadow: 0px 4px 20px 0px #6a6a6a1a;
}
p {
  font-weight: 400;
}
.inputs {
  width: 268px;
  height: 42px;
}
.inputs input {
  padding: 12px 0 12px 12px;
}
.chevon-input {
  right: 12;
}
.icons {
  padding-right: 12px;
}
.room-group {
  gap: 20px;
}
.barrier {
  margin: 36px 0;
  height: 2px;
  background-color: #eeeeee;
}
.home-link {
  color: #9e9e9e;
}
.sidebar {
  min-height: 100vh;
  box-shadow: 4px 0px 20px 0px rgba(106, 106, 106, 0.1);
}
</style>
