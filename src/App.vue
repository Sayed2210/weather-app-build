<template>
  <div class="bg-cyan-900">
    <MainNav />
    <RouterView class="flex-1" v-slot="{ Component }">
      <Transition name="fade">
        <component :is="Component" />
      </Transition>
    </RouterView>
  </div>
</template>

<script setup>
import MainNav from "./components/MainNav.vue";
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
