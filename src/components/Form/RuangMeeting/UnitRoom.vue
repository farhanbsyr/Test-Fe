<template>
  <div class="units d-flex gap-3">
    <!-- unit -->
    <div class="unit-room">
      <div class="input-group d-flex flex-column gap-2">
        <label class="font-size-large fw-semibold" for="unit">Unit</label>
        <div
          @click="isMenuOpen = !isMenuOpen"
          class="inputs rounded-3 position-relative bg-transparent border-secondary border border-1 border-black d-flex align-items-center"
        >
          <input
            readonly
            required
            type="text"
            name="unit"
            id="unit"
            :placeholder="loadingUnit ? 'Loading...' : 'Pilih Unit'"
            v-model="unit"
            class="h-100 flex-grow-1 border-secondary rounded-3 border-0 font-size-large"
          />
          <div class="icons d-flex justify-content-center align-items-center">
            <img
              class="chevon-input"
              src="../../../assets/images/chevron-input.svg"
              alt="dropdown"
            />
          </div>

          <!-- toogle -->
          <div
            v-if="isMenuOpen"
            class="position-absolute border border-1 border-black w-100 toogle-input"
          >
            <ul class="d-flex flex-column gap-2 list-unstyled m-0">
              <li
                class="list-unit px-2"
                @click="handleUnit(unit.officeName)"
                v-for="unit in unitData"
                :key="unit.id"
              >
                {{ unit.officeName }}
              </li>
            </ul>
          </div>
        </div>
        <span v-if="isUnitValid" class="text-danger font-size-large fw-semibold"
          >This field cannot be empty</span
        >
      </div>
    </div>
    <!-- end unit -->

    <!-- ruang meeting -->
    <div class="room-meeting">
      <div class="input-group d-flex flex-column gap-2">
        <label class="font-size-large fw-semibold" for="roomMeeting"
          >Ruang Meeting</label
        >
        <div
          @click="isRoomOpen = !isRoomOpen"
          class="inputs rounded-3 bg-transparent border-secondary border border-1 border-black d-flex align-items-center"
        >
          <input
            readonly
            required
            type="text"
            name="roomMeeting"
            id="roomMeeting"
            v-model="room"
            :placeholder="loadingRoom ? 'Loading....' : 'Pilih Ruang Meeting'"
            class="h-100 flex-grow-1 border-secondary rounded-3 border-0 font-size-large"
          />
          <div class="icons d-flex justify-content-center align-items-center">
            <img
              class="chevon-input"
              src="../../../assets/images/chevron-input.svg"
              alt="dropdown"
            />
          </div>
          <div
            v-if="isRoomOpen"
            class="position-absolute border border-1 border-black w-100 toogle-input"
          >
            <ul class="d-flex flex-column gap-2 list-unstyled m-0">
              <li
                class="list-unit px-2"
                @click="handleRoom(room.roomName)"
                v-for="room in filteredRoom"
                :key="room.id"
              >
                {{ room.roomName }}
              </li>
            </ul>
          </div>
        </div>
        <span v-if="isRoomValid" class="text-danger font-size-large fw-semibold"
          >This field cannot be empty</span
        >
      </div>
    </div>
    <!-- end ruang meeting -->
  </div>
</template>

<script setup>
import axios from "axios";
import { computed, onBeforeMount, onMounted, ref, watch } from "vue";

const emit = defineEmits(["update:capacity", "update:unit", "update:room"]);
const isMenuOpen = ref(false);
const isRoomOpen = ref(false);
const isUnitValid = ref(false);
const isRoomValid = ref(false);
const unitData = ref([]);
const roomData = ref([]);
const unit = ref("");
const room = ref("");
const capacityData = ref(0);
const loadingUnit = ref(false);
const loadingRoom = ref(false);

const handleUnit = (msg) => {
  unit.value = msg;
  emit("update:unit", unit.value);
};

const handleRoom = (msg) => {
  room.value = msg;
  emit("update:room", room.value);
};

const filteredRoom = computed(() =>
  roomData.value.filter(
    (item) => item.officeName.toLowerCase() == unit.value.toLowerCase()
  )
);

watch(unit, (newUnit) => {
  getRoom();

  // Check if the current room is valid for the new unit
  const isRoomValidForUnit = filteredRoom.value.some(
    (roomItem) => roomItem.roomName.toLowerCase() === room.value.toLowerCase()
  );

  if (!isRoomValidForUnit) {
    // If no room matches the current unit, reset room
    room.value = "";
    emit("update:room", room.value);
  }
});

const filteredCapacity = computed(
  () =>
    roomData.value.find(
      (item) => item.roomName.toLowerCase() === room.value.toLowerCase()
    )?.capacity || 0
);

watch(filteredCapacity, (newCapacity) => {
  capacityData.value = newCapacity;
  emit("update:capacity", newCapacity);
});

onBeforeMount(() => {
  getUnit();
});

async function getUnit() {
  try {
    loadingUnit.value = true;
    const response = await axios.get(
      "https://6666c7aea2f8516ff7a4e261.mockapi.io/api/dummy-data/masterOffice"
    );
    if (response.status === 200 || response.status === 201) {
      return (unitData.value = response.data);
    } else {
      throw new Error("Failed to Get Data Unit");
    }
  } catch (error) {
    console.error("Error during get data:", error);
    throw error;
  } finally {
    loadingUnit.value = false;
  }
}

async function getRoom() {
  try {
    loadingRoom.value = true;
    const response = await axios.get(
      "https://6666c7aea2f8516ff7a4e261.mockapi.io/api/dummy-data/masterMeetingRooms"
    );
    if (response.status === 200 || response.status === 201) {
      return (roomData.value = response.data);
    } else {
      throw new Error("Failed to Get Data Room");
    }
  } catch (error) {
    console.error("Error during get Room:", error);
    throw error;
  } finally {
    loadingRoom.value = false;
  }
}
</script>

<style scoped>
.icons {
  padding-right: 12px;
}
</style>
