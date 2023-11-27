<template>
  <Transition name="fade">
    <div
      v-show="showModel"
      class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8"
    >
      <Transition name="fade"
        ><div
          v-if="showModel"
          class="p-4 bg-white self-start mt-32 max-w-screen-md"
        >
          <slot />
          <button
            class="text-white mt-8 bg-cyan-500 rounded-lg py-2 px-6"
            @click="$emit('close-model')"
          >
            Close
          </button>
        </div></Transition
      >
    </div>
  </Transition>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
defineEmits(["close-model"]);
defineProps({
  showModel: {
    type: Boolean,
    default: false,
  },
});
</script>

<style lang="scss" scoped>
// .v-enter-active,
// .v-leave-active {
//   transition: opacity 0.5s ease;
// }

// .v-enter-from,
// .v-leave-to {
//   opacity: 0;
// }

// transition animations
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
