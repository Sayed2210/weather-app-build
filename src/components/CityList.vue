<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>
  <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in the field above.
  </p>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import CityCard from "./CityCard.vue";
import { useRouter } from "vue-router";
const router = useRouter();

const savedCities = ref([]);

const getcities = async () => {
  //check if there is data in localstorage
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
    //loop all data in local storage
    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
        )
      );
    });
    const weatherData = await Promise.all(requests);
    await new Promise((res) => setTimeout(res, 800));
    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
    console.log(savedCities.value);
  }
};
await getcities();

//go to city route
const goToCityView = (city) => {
  router.push({
    name: "cityView",
    params: { state: city.state, city: city.city },
    query: { lat: city.coords.lat, lng: city.coords.lng, id: city.id },
  });
};
</script>
