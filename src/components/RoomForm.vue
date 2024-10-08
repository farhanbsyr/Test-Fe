<script setup>
import SideBar from "@/components/SideBar.vue";
import UnitRoom from "@/components/Form/RuangMeeting/UnitRoom.vue";
import { RouterLink } from "vue-router";
import DetailDateVue from "./Form/DetailDate.vue";

import Title from "@/components/Title.vue";
import { computed, onMounted, watch, reactive, ref, onBeforeMount } from "vue";
import axios from "axios";
import Navbar from "@/components/Navbar.vue";

const capacityUpt = ref(0);
const participantCount = ref(0);
const snackData = ref([]);
const meetingStartTime = ref(0);
const meetingEndTime = ref(0);
const totalAmount = ref(0);
const selectedSnacks = ref([]);
const unit = ref("");
const room = ref("");
const dateEmit = ref("");
const errors = reactive({
  capacity: "",
  participantCount: "",
  startMeeting: "",
  endMeeting: "",
});
const validateForm = () => {
  errors.capacity = capacityUpt.value ? "" : "Kapasitas harus diisi";
  errors.participantCount = participantCount.value
    ? ""
    : "Jumlah peserta harus diisi";
  errors.startMeeting = meetingStartTime.value
    ? ""
    : "Waktu mulai rapat harus diisi";
  errors.endMeeting = meetingEndTime.value
    ? ""
    : "Waktu selesai rapat harus diisi";

  // Tambahkan validasi tambahan sesuai kebutuhan
  if (participantCount.value > capacityUpt.value) {
    errors.participantCount = "Jumlah peserta tidak boleh lebih dari kapasitas";
  }
  if (
    meetingStartTime.value &&
    meetingEndTime.value &&
    meetingEndTime.value < meetingStartTime.value
  ) {
    errors.endMeeting = "Waktu selesai rapat tidak boleh sebelum waktu mulai";
  }
};

const handleCapacity = (msg) => {
  capacityUpt.value = msg;
};
const handleUnitUpdate = (msg) => {
  unit.value = msg;
};
const handleRoomUpdate = (msg) => {
  room.value = msg;
};

function calculateTotalAmount() {
  totalAmount.value = snackData.value
    .filter((snack) => selectedSnacks.value.includes(snack.name))
    .reduce((total, snack) => total + snack.maxPrice, 0);
}

const handleStartMeeting = (msg) => {
  meetingStartTime.value = parseInt(msg);
};

const handleFinishMeeting = (msg) => {
  meetingEndTime.value = parseInt(msg);
};
const handleDate = (msg) => {
  dateEmit.value = parseInt(msg);
};

const validateSnacks = (snackName) => {
  if (!meetingStartTime.value) return false;
  if (!meetingEndTime.value) return false;
  if (meetingEndTime.value < meetingStartTime.value) {
    return false;
  }
  const isStart = meetingStartTime.value;
  const isFinish = meetingEndTime.value;

  if (isStart <= 11 && isFinish > 14) {
    // Sebelum jam 11:00
    return (
      snackName === "Snack Siang" ||
      snackName === "Makan Siang" ||
      snackName === "Snack Sore"
    );
  } else if (isStart <= 11 && isFinish <= 11) {
    // Setelah jam 14:00
    return snackName === "Snack Siang";
  } else if (isStart <= 11 && isFinish <= 14) {
    // Antara jam 11:00 dan 14:00
    return snackName === "Makan Siang" || snackName === "Snack Siang";
  } else if (isStart >= 14 && isFinish > 14) {
    return snackName === "Snack Sore";
  } else if (isStart >= 11 && isFinish > 14) {
    return snackName === "Makan Siang" || snackName === "Snack Sore";
  } else if (isStart >= 11 && isFinish <= 14) {
    return snackName === "Makan Siang";
  }
  return false;
};

const isParticipantCountValid = computed(() => {
  return participantCount.value <= capacityUpt.value;
});

const amount = computed(() => {
  return totalAmount.value * participantCount.value;
});

onBeforeMount(() => {
  getSnack();
});

onMounted(() => {
  watch(selectedSnacks, (newValue) => {
    calculateTotalAmount();
  });
});
watch([meetingStartTime, meetingEndTime], (value) => {
  snackData.value.forEach((snack) => {
    snack.isDisabled = !validateSnacks(snack.name);
  });
});

const handleSubmit = () => {
  validateForm();
  if (Object.values(errors).every((error) => !error)) {
    // Simulasi pengiriman data atau proses
    console.log("Form submitted successfully");

    // Reset semua inputan
    capacityUpt.value = 0;
    participantCount.value = 0;
    meetingStartTime.value = 0;
    meetingEndTime.value = 0;
    selectedSnacks.value = [];
    unit.value = "";
    room.value = "";
    dateEmit.value = "";

    // Mengosongkan data snack
    snackData.value = [];

    // Reset total amount
    totalAmount.value = 0;

    // Opsional: Berikan feedback kepada pengguna
    alert("Form berhasil disubmit!");
  } else {
    // Jika ada error, tampilkan feedback atau lakukan tindakan lainnya
    console.error("Form contains errors.");
  }
};

async function getSnack() {
  try {
    const response = await axios.get(
      "https://6686cb5583c983911b03a7f3.mockapi.io/api/dummy-data/masterJenisKonsumsi"
    );
    if (response.status === 200 || response.status === 201) {
      snackData.value = response.data.map((snack) => ({
        ...snack,
        isDisabled: !validateSnacks(snack.name),
      }));
    } else {
      throw new Error("Failed to Get Data Unit");
    }
  } catch (error) {
    console.error("Error during get data:", error);
    throw error;
  }
}
</script>

<template>
  <form
    @submit="handleSubmit"
    class="wrapper-room d-flex flex-column border border-1 rounded-2"
  >
    <!-- room -->
    <div class="room-group d-flex flex-column gap-3">
      <h3 class="fs-6 fw-semibold m-0">Informasi Ruang Meeting</h3>
      <UnitRoom
        @update:capacity="handleCapacity"
        @update:unit="handleUnitUpdate"
        @update:room="handleRoomUpdate"
      />

      <!-- kapasitas -->
      <div class="kapasistas">
        <div class="input-group d-flex flex-column gap-2">
          <label class="font-size-large fw-semibold" for="capacity"
            >Kapasitas</label
          >
          <div
            class="inputs rounded-3 z-1 bg-transparent border-0 d-flex align-items-center"
          >
            <input
              required
              disabled
              type="text"
              name="capacity"
              id="capacity"
              v-model="capacityUpt"
              class="h-100 text-black flex-grow-1 z-3 rounded-3 border-0 font-size-large"
            />
          </div>
        </div>
      </div>
    </div>

    <p class="w-100 m-0 barrier border"></p>

    <!-- meeting group -->
    <div class="detail-meeting">
      <h3 class="fs-6 fw-semibold mb-3">Informasi Rapat</h3>
      <div class="meeting-group d-flex flex-column gap-4">
        <DetailDateVue
          @update:startMeeting="handleStartMeeting"
          @update:finishMeeting="handleFinishMeeting"
          @update:date="handleDate"
        />

        <!-- Jumlah Peserta -->
        <div class="jumlah peserta">
          <div class="input-group d-flex flex-column gap-2">
            <label class="font-size-large fw-semibold" for="participant"
              >Jumlah Peserta</label
            >

            <div class="inputs">
              <input
                required
                type="number"
                name="participant"
                id="participant"
                v-model.number="participantCount"
                placeholder="Masukan Jumlah Peserta"
                class="w-100 h-100 border-secondary rounded-3 border-1 font-size-large"
              />
            </div>
            <p
              v-if="!isParticipantCountValid"
              class="error-message text-danger fw-semibold font-size-medium m-0"
            >
              Jumlah peserta tidak boleh lebih dari kapasitas.
            </p>
          </div>
        </div>

        <!-- Konsumsi -->
        <div class="konsumsi">
          <h3 class="font-size-large fw-semibold">Jenis Konsumsi</h3>
          <div v-for="snack in snackData" :key="snack.id" class="form-check">
            <input
              class="form-check-input disabled"
              type="checkbox"
              :disabled="!validateSnacks(snack.name)"
              :value="snack.name"
              v-model="selectedSnacks"
              :id="snack.name.trim().toLowerCase()"
            />
            <label
              class="form-check-label"
              :for="snack.name.trim().toLowerCase()"
              >{{ snack.name }}</label
            >
          </div>
        </div>

        <!-- Total Konsumsi -->
        <div class="total-konsumsi">
          <label for="amountKonsumsi" class="font-size-large fw-semibold mb-2"
            >Nominal Konsumsi</label
          >
          <div class="input-group inputs mb-3">
            <span class="input-group-text" id="basic-addon1">Rp.</span>
            <input
              disabled
              required
              type="number"
              name="amountKonsumsi"
              id="amountKonsumsi"
              class="form-control"
              v-model="amount"
              aria-label="Konsumsi"
              aria-describedby="basic-addon1"
            />
          </div>
        </div>
      </div>
    </div>

    <p class="w-100 m-0 barrier border"></p>

    <div class="wrapper-btn d-flex gap-3 justify-content-end">
      <button class="btn cancel bg-white text-danger font-size-large">
        batal
      </button>
      <button
        type="submit"
        class="btn submit btn-primary text-white font-size-large"
      >
        Submit
      </button>
    </div>
  </form>
</template>

<style scoped>
.wrapper-title {
  margin-bottom: 32px;
}
.wrapper-room {
  padding: 20px 28px;
  border-color: #eeeeee;
  gap: 36px;
  box-shadow: 0px 4px 20px 0px #6a6a6a1a;
}
p {
  font-weight: 400;
}

.icons {
  padding-right: 12px;
}
.room-group {
  gap: 20px;
}
.barrier {
  border: 2px;
  background-color: #eeeeee;
}

.home-link {
  color: #9e9e9e;
}
.wrapper-btn button.cancel:hover {
  background-color: #dc2626;
  color: white;
}
#capacity {
  background: #d3d3d3;
}
</style>
