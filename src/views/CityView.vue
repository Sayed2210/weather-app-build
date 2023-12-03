<template>
  <div class="bg-cyan-900">
    <Suspense>
      <AsyncCity />
      <!-- loading state via #fallback slot -->
      <template #fallback> <CityViewSkeleton /> </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCity from "@/components/AsyncCity.vue";
import CityViewSkeleton from "@/components/CityViewSkeleton.vue";
</script>

<style lang="scss" scoped></style>
