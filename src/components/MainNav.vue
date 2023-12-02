<template>
  <header class="bg-cyan-500 shadow-lg sticky top-0 px-2 md:px-0">
    <div class="container mx-auto">
      <nav class="flex items-center justify-between py-5">
        <router-link :to="{ name: 'home' }" class="text-2xl text-white"
          ><font-awesome-icon :icon="['fas', 'cloud-sun']" />The Local
          Weather</router-link
        >
        <div class="icons">
          <font-awesome-icon
            :icon="['fas', 'info']"
            @click="toggleModel"
            class="text-xl hover:text-cyan-500 mr-2 duration-150 cursor-pointer text-white bg-cyan-900 p-2 rounded-full w-3.5 h-3.5"
          />
          <font-awesome-icon
            :icon="['fas', 'plus']"
            @click="addCity()"
            v-if="route.query.preview"
            class="text-xl hover:text-cyan-500 duration-150 cursor-pointer text-white bg-cyan-900 p-2 rounded-full w-3.5 h-3.5"
          />
        </div>
        <BaseModel :showModel="showModel" @close-model="toggleModel">
          <div class="text-black">
            <h1 class="text-2xl mb-1 font-medium">About:</h1>
            <p class="mb-4 text-gray-500">
              The Local Weather allows you to track the current and future
              weather of cities of your choosing.
            </p>
            <h2 class="text-2xl font-medium">How it works:</h2>
            <ol class="list-decimal list-inside mb-4">
              <li class="text-gray-500">
                Search for your city by entering the name into the search bar.
              </li>
              <li class="text-gray-500">
                Select a city within the results, this will take you to the
                current weather for your selection.
              </li>
              <li class="text-gray-500">
                Track the city by clicking on the "+" icon in the top right.
                This will save the city to view at a later time on the home
                page.
              </li>
            </ol>

            <h2 class="text-2xl font-medium">Removing a city</h2>
            <p class="text-gray-500">
              If you no longer wish to track a city, simply select the city
              within the home page. At the bottom of the page, there will be am
              option to delete the city.
            </p>
          </div></BaseModel
        >
      </nav>
    </div>
  </header>
</template>

<script setup>
import BaseModel from "./BaseModel.vue";
import { uid } from "uid";
import { ref } from "vue";
import { useRoute, useRouter, RouterLink } from "vue-router";
const route = useRoute();
const router = useRouter();
const showModel = ref(null);

const savedCities = ref([]);
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }
  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };
  //set dat to localstorage
  savedCities.value.push(locationObj);
  localStorage.setItem("savedCities", JSON.stringify(savedCities.value));
  //reset all queries
  let query = Object.assign({}, route.query);
  delete query.preview;
  router.replace({ query });
};

const toggleModel = () => {
  showModel.value = !showModel.value;
};
</script>

<style lang="scss" scoped></style>
