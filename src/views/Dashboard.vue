<template>
  <div>
    <nav class="d-flex wrapper-dashboard">
      <div>
        <img src="../assets/images/logo-dashboard.svg" alt="" />
      </div>
      <h1 class="font-size-larger fw-bold">DASHBOARD</h1>
    </nav>
    <div class="content gap-4 d-flex flex-column wrapper-content">
      <div class="filter-mounth">
        <div class="btn-group">
          <button
            class="btn inputs d-flex justify-content-between align-items-center border-1 border-success dropdown-toggle"
            type="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
          >
            Select Bulan
          </button>
          <ul class="dropdown-menu w-100">
            <li @click="toogleMonth = false">
              <a class="dropdown-item" href="#">Januari</a>
            </li>
            <li @click="toogleMonth = true">
              <a class="dropdown-item" href="#">Febuari</a>
            </li>
          </ul>
        </div>
      </div>
      <div v-for="data in usedData" :key="data.id" class="row">
        <div v-for="(item, index) in data.data" :key="index + 1" class="col">
          <div class="title-unit align-items-center d-flex gap-2">
            <div class="icon-unit d-flex justify-content-center">
              <img src="../assets/images/unit-icon.svg" alt="icon unit" />
            </div>
            <h1 class="fs-6 m-0">{{ item.officeName }}</h1>
          </div>
          <div
            v-for="(room, index) in item.detailSummary"
            :key="index + 1"
            class="card-room bg-dasar rounded-2 d-flex flex-column gap-1"
          >
            <div class="title-room font-size-large">{{ room.roomName }}</div>
            <div class="presentas row">
              <div class="text-jumlah col-8">
                <p class="m-0 font-size-small">Persentase Pemakaian</p>
                <h1 class="font-size-larger fw-bolder">
                  {{
                    (room.averageOccupancyPerMonth / room.capacity).toFixed(2)
                  }}
                </h1>
              </div>
              <div class="col-4 font-size-smaller">Diagramnya</div>
            </div>
            <div
              v-for="(amount, index) in room.totalConsumption"
              :key="index + 1"
              class="nominal"
            >
              <p class="m-0 font-size-small">Nominal Konsumsi</p>
              <h1 class="font-size-larger fw-bolder">
                {{ amount.totalPrice }}
              </h1>
            </div>
            <div
              v-for="(snack, index) in room.totalConsumption"
              :key="index + 1"
              class="amount-snack d-flex flex-column gap-1"
            >
              <div class="snack-group">
                <h5 class="font-size-small fw-semibold">{{ snack.name }}</h5>
                <div class="amount">
                  <p class="font-size-medium">{{ snack.totalPackage }}</p>
                  <div class="pricing"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { computed, onMounted, ref } from "vue";

const toogleMonth = ref(false);
const usedData = ref([]);

onMounted(() => {
  getUsed();
  console.log(usedData);
});
async function getUsed() {
  try {
    const response = await axios.get(
      "https://6686cb5583c983911b03a7f3.mockapi.io/api/dummy-data/summaryBookings"
    );
    if (response.status === 200 || response.status === 201) {
      return (usedData.value = response.data);
    } else {
      throw new Error("Failed to Get Data Unit");
    }
  } catch (error) {
    console.error("Error during get data:", error);
    throw error;
  } finally {
    console.log(usedData.value);
  }
}
const filteredData = computed(() => {
  if (toogleMonth.value) {
    // Return hanya data dengan id === 2
    return usedData.value.filter((data) => data.id === 2);
  } else {
    // Return semua data jika toogleMonth adalah false
    return usedData.value.filter((data) => data.id === 1);
  }
});
</script>

<style scoped>
.wrapper-dashboard {
  padding: 12px 20px;
}
.wrapper-content {
  padding: 20px;
}
.title-unit h1 {
  color: #868e96;
}
.icon-unit {
  width: 24px;
  height: 24px;
}
</style>
