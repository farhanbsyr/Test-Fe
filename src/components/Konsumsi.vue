<template>
  <div class="konsumsi">
    <h3 class="font-size-large fw-semibold">Jenis Konsumsi</h3>
    <div v-for="snack in snackData" :key="snack.id" class="form-check">
      <input
        class="form-check-input"
        type="checkbox"
        :value="snack.name"
        :id="snack.name.trim().toLowerCase()"
      />
      <label class="form-check-label" :for="snack.name.trim().toLowerCase()">{{
        snack.name
      }}</label>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { onBeforeMount, onMounted, ref } from "vue";
const snackData = ref([]);

onMounted(() => {
  getSnack();
});

async function getSnack() {
  try {
    const response = await axios.get(
      "https://6686cb5583c983911b03a7f3.mockapi.io/api/dummy-data/masterJenisKonsumsi"
    );
    if (response.status === 200 || response.status === 201) {
      return (snackData.value = response.data);
    } else {
      throw new Error("Failed to Get Data Unit");
    }
  } catch (error) {
    console.error("Error during get data:", error);
    throw error;
  } finally {
    console.log(snackData.value);
  }
}
</script>

<style></style>
