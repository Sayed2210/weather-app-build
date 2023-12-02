<template>
  <div class="bg-cyan-900">
    <Suspense>
      <AsyncCity />
      <!-- loading state via #fallback slot -->
      <template #fallback> Loading... </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCity from "@/components/AsyncCity.vue";
</script>

<style lang="scss" scoped></style>
