<template>
  <header class="bg-cyan-500 shadow-lg sticky top-0">
    <div class="container mx-auto">
      <nav class="flex items-center justify-between py-5">
        <router-link :to="{ name: 'home' }" class="text-2xl text-white"
          ><font-awesome-icon :icon="['fas', 'cloud-sun']" />The Local
          Weather</router-link
        >
        <font-awesome-icon
          :icon="['fas', 'info']"
          class="text-xl hover:text-cyan-500 duration-150 cursor-pointer text-white bg-cyan-900 p-2 rounded-full w-3.5 h-3.5"
        />
      </nav>
    </div>
  </header>
</template>

<script setup></script>

<style lang="scss" scoped></style>
