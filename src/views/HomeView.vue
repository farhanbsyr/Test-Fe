<script setup>
import Title from "@/components/Title.vue";
import RoomForm from "@/components/RoomForm.vue";
import { ref } from "vue";
const isForm = ref(false);
const handleIsFormUpdate = (msg) => {
  isForm.value = msg;
};
</script>

<template>
  <div class="wrapper-meeting w-100">
    <!-- title -->
    <Title @update:isForm="handleIsFormUpdate" class="wrapper-title" />
    <!-- end Title -->
    <room-form v-if="isForm" />
  </div>
</template>

<style scoped>
.wrapper-meeting {
  padding: 24px 28px 36px 0px;
}
.wrapper-title {
  margin-bottom: 32px;
}
</style>
