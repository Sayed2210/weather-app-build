<template>
  <div
    class="flex items-center flex bg-cyan-500 px-2 mt-1 py-2 rounded-md shadow-md"
  >
    <div class="flex flex-col flex-1 gap-2">
      <AnimatedPlaceholder class="w-[50%]" />
      <AnimatedPlaceholder class="w-[40%]" />
    </div>
    <div class="flex flex-col flex-1 gap-2 items-end">
      <AnimatedPlaceholder class="max-w-[50px] w-full" />
      <AnimatedPlaceholder class="max-w-[40px] w-full" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>
