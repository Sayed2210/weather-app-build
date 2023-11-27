<template>
  <main class="home bg-cyan-900 h-screen text-white">
    <div class="pt-4 mb-8 container mx-auto">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="search"
        placeholder="Search For a city or state"
        class="py-2 px-1 w-full bg-transparent border-b border-white focus:outline-none focus:border-cyan-500 focus:shadow-md"
      />
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
const apiKey =
  "pk.eyJ1Ijoic2Fpa284NSIsImEiOiJjbHBoY3RieWEwOWgwMnFwMGFoNjRlbHhqIn0.xKEp3xi0P0UNmX8ZzQwA_A";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${apiKey}&types=place`
      );
      mapboxResults.value = result.data.features;
      console.log(mapboxResults.value);
      return;
    }
    mapboxResults.value = null;
    console.log(mapboxResults.value);
  }, 300);
};
</script>

<style lang="scss" scoped>
input::placeholder {
  // color: rgba(6, 182, 212, 1);
  color: white;
}
input:focus::placeholder {
  color: rgba(6, 182, 212, 1);
  // color: white;
}
</style>
