

<template>
  <Valid v-if="showCarNo" />
  <CarForm v-if="!showCarNo" :isMain="isMain" />
</template>
<script setup>
import Valid from "./components/valid.vue";
import CarForm from "./components/form.vue";
import { ref, onMounted } from "vue";
let showCarNo = ref(false);
let isMain = ref(false);
onMounted(() => {
  const carNo = localStorage.getItem("carNo");
  let href = window.location.href;
  isMain.value = href.includes("main");

  if (isMain.value) {
    return;
  }
  if (!carNo) {
    showCarNo.value = true;
  } else {
    let href = window.location.href;

    debugger;
  }
});
</script>
<style scoped>
</style>
