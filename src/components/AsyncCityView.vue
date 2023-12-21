<template>
    <div>
        <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
        <!-- Banner -->
        <p>
            You are currently previewing this city, click the "+" button
            to track.
        </p>
        </div>
        <!-- Weather  Overview -->
        <div class="flex flex-col items-center text-white py-12">

        <h1 class="text-4xl mb-2">{{ route.params.city }}, {{ route.params.state }} </h1>
        <p class="text-sm mb-12">
            <!-- {{
          new Date(weatherData.currentTime).toLocaleDateString(
            "en-us",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
          )
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString(
            "en-us",
            {
              timeStyle: "short",
            }
          )
        }} -->

        </p>

        </div>
        </div>
    </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
const openWeatherAPIKey = '6334597926f131b0bc4fba671c654c0f';

const route = useRoute();

const getWeatherData = async () => {
    try{
        console.log('Malking api call from getweatherdata function, lat&log ', route.query.lat, ' ', route.query.log);
         //Below is openweatherapi call WITHOUT geocoding so calling by name here https://openweathermap.org/current#data
        const weatherData = await axios.get(
                //TODO THIS WONT WORK AS IN TUTORIAL
                // in tutorial he uses the onecall api which contains time data as well, as of this day this is payable
                // if I want to do this I need to request time and date information from another place 
                `https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.log}&exclude={part}&appid=6334597926f131b0bc4fba671c654c0f&units=metric`
        );
      // cal current date & time
      const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime =
        utc + 1000 * weatherData.data.timezone_offset;
    });

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();
</script>

