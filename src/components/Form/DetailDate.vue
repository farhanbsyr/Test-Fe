<template>
  <div class="detail-date d-flex gap-3">
    <!-- tanggal rapat -->
    <div class="date-room">
      <div class="input-group d-flex flex-column gap-2">
        <label class="font-size-large fw-semibold" for="date"
          >Tanggal Rapat</label
        >
        <div
          class="inputs rounded-3 bg-transparent border-secondary border border-1 border-black d-flex align-items-center"
        >
          <input
            required
            type="date"
            name="date"
            id="date"
            v-model="date"
            :min="minDate"
            placeholder="Pilih unit"
            class="h-100 flex-grow-1 border-secondary rounded-3 border-0 font-size-large"
          />
        </div>
      </div>
    </div>
    <!-- end tanggal rapat -->

    <!-- Waktu Mulai -->
    <div class="room-meeting">
      <div class="input-group d-flex flex-column gap-2">
        <label class="font-size-large fw-semibold" for="startTime"
          >Waktu Mulai</label
        >
        <div
          @click="isTimeStart = !isTimeStart"
          class="inputs position-relative rounded-3 bg-transparent border-secondary border border-1 border-black d-flex align-items-center"
        >
          <input
            required
            type="text"
            name="startTime"
            id="startTime"
            v-model="startMeeting"
            placeholder="Pilih Waktu Mulai"
            class="h-100 modal-dialog flex-grow-1 border-secondary rounded-3 border-0 font-size-large"
          />
          <div class="icons d-flex justify-content-center align-items-center">
            <img
              class="chevon-input"
              src="../../assets/images/chevron-input.svg"
              alt="dropdown"
            />
          </div>
          <div
            v-if="isTimeStart"
            class="position-absolute border border-1 border-black w-100 toogle-input"
          >
            <ul class="d-flex flex-column gap-2 list-unstyled m-0">
              <li
                class="list-unit px-2 scrollspy-example"
                @click="handleStartMeeting(time)"
                v-for="(time, index) in meetingTime"
                :key="index"
              >
                {{ time }}
              </li>
            </ul>
            <p v-if="startTimeError" class="error-message">
              {{ startTimeError }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- end Waktu Mulai -->

    <!-- Waktu Selesai -->
    <div class="room-meeting">
      <div class="input-group d-flex flex-column gap-2">
        <label class="font-size-large fw-semibold" for="endTime"
          >Waktu Selesai</label
        >
        <div
          @click="isTimeFinish = !isTimeFinish"
          class="inputs position-relative rounded-3 bg-transparent border-secondary border border-1 border-black d-flex align-items-center"
        >
          <input
            required
            type="text"
            name="endTime"
            id="endTime"
            v-model="finishMeeting"
            placeholder="Pilih Waktu Selesai"
            class="h-100 flex-grow-1 border-secondary rounded-3 border-0 font-size-large"
          />
          <div class="icons d-flex justify-content-center align-items-center">
            <img
              class="chevon-input"
              src="../../assets/images/chevron-input.svg"
              alt="dropdown"
            />
          </div>
          <div
            v-if="isTimeFinish"
            class="position-absolute border border-1 border-black w-100 toogle-input"
          >
            <ul class="d-flex flex-column gap-2 list-unstyled m-0">
              <li
                class="list-unit px-2"
                @click="handleFinishMeeting(time)"
                v-for="(time, index) in meetingTime"
                :key="index"
              >
                {{ time }}
              </li>
            </ul>
          </div>
        </div>
        <p
          v-if="!isFinishTimeValid"
          class="error-message text-danger fw-semibold font-size-medium m-0"
        >
          Waktu selesai tidak boleh lebih kecil dari waktu mulai.
        </p>
      </div>
    </div>
    <!-- ends Waktu Selesai -->
  </div>
</template>

<script setup>
import axios from "axios";
import { computed, watch, defineEmits, onBeforeMount, ref } from "vue";

const emit = defineEmits([
  "update:startMeeting",
  "update:finishMeeting",
  "update:date",
]);

const isTimeStart = ref(false);
const isTimeFinish = ref(false);
const startMeeting = ref(0);
const finishMeeting = ref(0);
const date = ref("");
const meetingTime = ref([
  "10.00",
  "11.00",
  "12.00",
  "13.00",
  "14.00",
  "15.00",
  "16.00",
  "17.00",
]);
const handleFinishMeeting = (msg) => {
  finishMeeting.value = msg;
};
const handleStartMeeting = (msg) => {
  startMeeting.value = msg;
};

const isFinishTimeValid = computed(() => {
  if (!parseInt(startMeeting.value) || !parseInt(finishMeeting.value))
    return true;
  return (
    parseFloat(parseInt(finishMeeting.value)) >=
    parseFloat(parseInt(startMeeting.value))
  );
});

const minDate = computed(() => {
  const today = new Date();
  const yyyy = today.getFullYear();
  const mm = String(today.getMonth() + 1).padStart(2, "0"); // Months are zero-based
  const dd = String(today.getDate()).padStart(2, "0");
  return `${yyyy}-${mm}-${dd}`;
});

watch(startMeeting, (newValue) => {
  emit("update:startMeeting", newValue);
});

watch(finishMeeting, (newValue) => {
  emit("update:finishMeeting", newValue);
});

watch(date, (newValue) => {
  emit("update:date", newValue);
});
</script>

<style scoped>
.icons {
  padding-right: 12px;
}
</style>
