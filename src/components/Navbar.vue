<template>
  <nav class="navbar z-3 w-100 navbar-expand-lg p-0 m-0 nav-bg">
    <div class="container-fluid mx-4 py-2 px-0">
      <div class="navbar-brand nav-left d-flex align-items-center p-0 m-0">
        <img
          src="../assets/images/IconPLN.png"
          width="38"
          height="52"
          alt="Icon PLN"
        />
        <h3 class="fs-6 text-white m-0" href="#">iMeeting</h3>
      </div>

      <!-- toogle btn -->
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="nav-right d-flex align-items-center gap-3">
        <div
          class="wrapper-icon d-flex justify-content-center align-items-center"
        >
          <img
            src="../assets/images/bell.svg"
            width="18"
            height="18"
            alt="Notification"
          />
        </div>
        <div class="profile">
          <img src="" class="bg-white rounded-circle" width="40" height="40" />
        </div>
        <h3 class="text-white font-size-large m-0">John Doe</h3>
        <div class="icons d-flex justify-content-center align-items-center">
          <img src="../assets/images/chevron-down.svg" alt="dropdown" />
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup></script>

<style scoped>
.nav-bg {
  background: linear-gradient(
    to right,
    rgba(24, 162, 186, 1),
    rgba(41, 99, 119, 1)
  );
}
.nav-left {
  gap: 20px;
}
</style>
