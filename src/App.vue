<script setup lang="ts">
import { onMounted } from "vue";

import { RouterView } from "vue-router";
import autofit from "autofit.js";

onMounted(() => {
  autofit.init(
    {
      dh: 1080,
      dw: 1920,
      el: "#app",
      resize: true,
    },
    false
  );
});
</script>

<template>
  <RouterView />
</template>

<style scoped>
</style>
