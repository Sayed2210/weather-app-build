<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Headline  -->
    <div
      v-if="route.query.preview"
      class="text-white text-center w-full bg-cyan-500 py-5"
    >
      <p>
        You are currently previewing this city, click the "+" icon to start
        tracking this city.
      </p>
    </div>
    <!-- Weather Overview -->
    <div class="flex flex-col items-center py-12 text-white">
      <h1 class="text-4xl mb-4 font-bold">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{
          new Date(weatherData.currentTime).toLocaleDateString("en-us", {
            weekday: "short",
            day: "2-digit",
            month: "long",
          })
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString("en-us", {
            timeStyle: "short",
          })
        }}
      </p>
      <p class="text-4xl mb-4">
        {{ Math.round(weatherData.current.temp) }} &deg;
      </p>
      <p class="mb-4">
        Feels Like {{ Math.round(weatherData.current.feels_like) }}
      </p>
      <p class="capitalize">{{ weatherData.current.weather[0].description }}</p>
      <img
        class="h-[150px] h-auto"
        :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
        alt=""
      />
    </div>
    <hr class="border-white border-opacity-10 border w-full" />
    <!-- hourly weather -->
    <div class="w-full py-12 max-w-screen-md">
      <div class="mx-8 text-white">
        <h1 class="mb-4">Hourly Weather</h1>
        <div class="flex gap-10 overflow-x-scroll">
          <div
            v-for="hourWeather in weatherData.hourly"
            :key="hourWeather.dt"
            class="flex flex-col gap-4 items-center"
          >
            <p>
              {{
                new Date(hourWeather.currentTime).toLocaleTimeString("en-us", {
                  hour: "numeric",
                })
              }}
            </p>
            <img
              class="w-auto h-[50px] object-cover"
              :src="`http://openweathermap.org/img/wn/${hourWeather.weather[0].icon}@2x.png`"
              alt=""
            />
            <p>{{ Math.round(hourWeather.temp) }}</p>
          </div>
        </div>
      </div>
    </div>
    <hr class="border-white border-opacity-10 border w-full" />
    <!-- weekly weather -->
    <div class="w-full py-12 max-w-screen-md">
      <div class="mx-8 text-white">
        <h2>Weekly Weather</h2>
        <div
          class="flex items-center"
          v-for="dayWeather in weatherData.daily"
          :key="dayWeather.dt"
        >
          <p class="flex-1">
            {{
              new Date(dayWeather.dt * 1000).toLocaleDateString("en-us", {
                weekday: "long",
              })
            }}
          </p>
          <img
            class="w-[50px] h-[50px] object-cover"
            :src="`http://openweathermap.org/img/wn/${dayWeather.weather[0].icon}@2x.png`"
            alt=""
          />
          <div class="flex gap-2 flex-1 justify-end">
            <p>Highest Deg: {{ Math.round(dayWeather.temp.max) }}</p>
            <p>Lowest Deg: {{ Math.round(dayWeather.temp.min) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`
    );
    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });
    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();
console.log(weatherData);
</script>

<style lang="scss" scoped></style>
