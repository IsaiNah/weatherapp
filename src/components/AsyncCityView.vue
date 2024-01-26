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
        <p class="text-sm mb-12">Date : 
       {{ formattedDate }}
        </p>
        <p class="text-sm mb-12">Time :
       {{ formattedTime }}
        </p>
        <p class="text-8xl mb-8">
            {{ Math.round(route.params.temp) }}&deg;

        </p>
        <p class="text-center">
            Feels like {{ Math.round(route.params.tempfeel) }}&deg;

        </p>
        

        <p class="capitalize">
            {{  weatherData.weather[0].main   }},  {{  weatherData.weather[0].description   }}
        </p>
        <img 
  v-if="weatherData && weatherData.weather && weatherData.weather[0].icon"
  class="w-[150px] h-auto"
  :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}.png`"
  alt=""
/>
</div>
<!-- {{ console.log('Icon is ', weatherData.weather[0].icon) }} -->

<hr class="border-white border-opacity-10 border w-full"/>

<!-- Hourly Weather -->
<div class="mx-w-screen-md w-full  py-12">
  <div class="mx-8 text-white">
    <h2 class="mb-4">Hourly Weather</h2>
    <div class="flex gap-10 overflow-x-scroll"></div>
  </div>
</div>

<div class="flex items-center gap-2 py-12 text-white cursor-pointer 
duration-150 hover:text-red-600"
 @click="removeCity"
>
  <p>Remove City</p>
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
                `https://api.openweathermap.org/data/2.5/weather?q=${route.params.city}&appid=${openWeatherAPIKey}&units=metric`
                //NOTE Hourly call not used due to credit card details needed incase of over 1000 api calls
               // 'https://api.openweathermap.org/data/3.0/onecall?lat=33.44&lon=-94.04&appid={openWeatherAPIKey}&units=metric'
        );

     

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();

// const router = useRouter();
// const removeCity = () => {};

//TODO REMOVE BELOW

    //extract timestemp
    const timestamp = weatherData.dt;
                 // Convert timestamp to a readable date and time
                  const date = new Date(weatherData.dt * 1000); // Convert from seconds to milliseconds
                  const  formattedDate = date.toLocaleDateString();
                  const  formattedTime = date.toLocaleTimeString();

                  const temp = weatherData.main.temp;
                  const feeltemp = weatherData.main.feels_like;

                  console.log(temp, ' ', feeltemp);

    
                  http://openweathermap.org/img/w/10d.png
                  console.log(`http://openweathermap.org/img/w/${weatherData.weather.icon}.png`);




</script>

