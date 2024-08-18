<template>
  <div>
    <nav class="d-flex align-items-center gap-3 wrapper-dashboard">
      <div>
        <img src="../assets/images/logo-dashboard.svg" alt="" />
      </div>
      <h1 class="font-size-larger fw-bold m-0">DASHBOARD</h1>
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
            {{
              loading
                ? "loading..."
                : monthT.length > 0
                ? monthT
                : "Select Month"
            }}
          </button>
          <ul class="dropdown-menu w-100">
            <li
              v-for="(month, index) in selectMonth"
              :key="index"
              @click="filteredMonth(index), handleMonth(month)"
            >
              <a class="dropdown-item" href="#">{{ month }}</a>
            </li>
          </ul>
        </div>
      </div>
      <div class="row">
        <div v-for="(item, index) in monthUsed" :key="index + 1" class="col">
          <div class="title-unit align-items-center d-flex gap-2 mb-1">
            <div class="icon-unit d-flex justify-content-center">
              <img src="../assets/images/unit-icon.svg" alt="icon unit" />
            </div>
            <h1 class="fs-6 m-0">{{ item.officeName }}</h1>
          </div>
          <div class="d-flex flex-column wrapper-room">
            <div
              v-for="(room, index) in item.detailSummary"
              :key="index + 1"
              class="card-room bg-dasar rounded-2 d-flex flex-column gap-1"
            >
              <div class="title-room font-size-large">{{ room.roomName }}</div>
              <div class="presentas row">
                <div class="text-jumlah col-8">
                  <p class="m-0 font-size-smaller dashboard-sec">
                    Persentase Pemakaian
                  </p>
                  <h1 class="font-size-larger fw-bolder">
                    {{
                      converPercent(
                        room.averageOccupancyPerMonth / room.capacity
                      )
                    }}
                  </h1>
                </div>
                <div class="col-4 font-size-smaller">Diagramnya</div>
              </div>
              <div class="nominal">
                <p class="m-0 font-size-small dashboard-sec">
                  Nominal Konsumsi
                </p>
                <h1 class="font-size-larger fw-bolder">
                  {{ formattedNominal(nominalTotal(room.totalConsumption)) }}
                </h1>
              </div>
              <div
                v-for="(snack, index) in room.totalConsumption"
                :key="index + 1"
                class="amount-snack d-flex flex-column gap-1"
              >
                <div class="snack-group row">
                  <h5
                    class="font-size-small m-0 align-self-center fw-semibold col"
                  >
                    {{ snack.name }}
                  </h5>
                  <div class="amount col">
                    <label for="mySlide" class="font-size-medium">{{
                      snack.totalPackage
                    }}</label>
                    <div class="pricing">
                      <input
                        type="range"
                        min="0"
                        :max="packageTotal(room.totalConsumption)"
                        :value="parseInt(snack.totalPackage)"
                        class="slider w-50"
                        id="mySlider"
                      />
                    </div>
                  </div>
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
import { onMounted, ref, watch } from "vue";

const usedData = ref([]);
const monthT = ref("");
const selectMonth = ["Januari", "Febuari"];
const monthUsed = ref([]);
const loading = ref(false);
const handleMonth = (value) => {
  monthT.value = value;
};

const filteredMonth = (month) => {
  if (month === "Januari") {
    monthUsed.value = usedData.value[0] ? usedData.value[0].data : [];
  } else if (month === "Febuari") {
    monthUsed.value = usedData.value[1] ? usedData.value[1].data : [];
  } else {
    monthUsed.value = [];
  }
};

watch(monthT, (newMonth) => {
  filteredMonth(newMonth);
});

const nominalTotal = (nominal) => {
  return nominal.reduce((total, item) => total + parseInt(item.totalPrice), 0);
};
const formattedNominal = (amount) => {
  return new Intl.NumberFormat("id-ID", {
    style: "currency",
    currency: "IDR",
  }).format(amount);
};

const packageTotal = (consumptions) => {
  return consumptions.reduce(
    (total, item) => total + parseInt(item.totalPackage),
    0
  );
};

const converPercent = (number) => {
  return `${(number * 100).toFixed(2)}%`;
};

onMounted(() => {
  getUsed();
});

async function getUsed() {
  try {
    loading.value = true;
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
    loading.value = false;
  }
}
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
.wrapper-room {
  gap: 12px;
}
.card-room {
  background-color: #f2f2f2;
  padding: 12px;
  gap: 12px;
}
/* slide */
</style>
