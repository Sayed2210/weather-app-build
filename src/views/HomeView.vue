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
      <ul
        v-if="mapboxResults"
        class="absoulute bg-cyan-500 text-white w-full shadow-md py-2 px-1 top[-66px]"
      >
        <p v-if="searchError" class="py-2 text-white">
          Sorry, something went wrong, please try again.
        </p>
        <p v-if="!searchError && mapboxResults.length === 0">
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            class="cursor-pointer py-2 px-1 hover:shadow-md"
            v-for="city in mapboxResults"
            :key="city.id"
            @click="cityPreview(city)"
          >
            {{ city.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";
// api key
const apiKey =
  "pk.eyJ1Ijoic2Fpa284NSIsImEiOiJjbHBoY3RieWEwOWgwMnFwMGFoNjRlbHhqIn0.xKEp3xi0P0UNmX8ZzQwA_A";
//search keyword
const searchQuery = ref("");
const queryTimeout = ref(null);
//data that we get from api
const mapboxResults = ref(null);
//handle error
const searchError = ref(null);

const router = useRouter();

//city preivew fun to make dynamic route and handle data from searched data
const cityPreview = (city) => {
  console.log(city);
  const [cityName, state] = city.place_name.split(",");
  router.push({
    name: "cityView",
    params: { state: state.trim(), city: cityName },
    query: {
      lat: city.geometry.coordinates[1],
      lng: city.geometry.coordinates[0],
    },
  });
};

//get data from api
const getSearchResults = () => {
  clearTimeout(queryTimeout);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${apiKey}&types=place`
        );
        mapboxResults.value = result.data.features;
      } catch (error) {
        searchError.value = true;
      }
      console.log(mapboxResults.value);
      return;
    }
    mapboxResults.value = null;
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
