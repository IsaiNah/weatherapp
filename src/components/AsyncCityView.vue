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

        <p>
            {{ 
                console.log('calling getWeatherData ', weatherData.dt) 

                
                
                
                
                }}
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
                `https://api.openweathermap.org/data/2.5/weather?q=${route.params.city}&appid=${openWeatherAPIKey}&units=metric`
        );

       // const response = weatherData.data;

        //extract timestemp
      //  const timestamp = response.dt;

        // Convert timestamp to a readable date and time
      //  const date = new Date(timestamp * 1000); // Convert from seconds to milliseconds
      //  formattedDate = date.toLocaleDateString();
      //  formattedTime = date.toLocaleTimeString();

//console.log('Current Date:', formattedDate);
//console.log('Current Time:', formattedTime);

       // console.log("timestamp ", timestamp);
    //   // cal current date & time
    //   const localOffset = new Date().getTimezoneOffset() * 60000;
    // const utc = weatherData.data.current.dt * 1000 + localOffset;
    // weatherData.data.currentTime =
    //   utc + 1000 * weatherData.data.timezone_offset;

    // // cal hourly weather offset
    // weatherData.data.hourly.forEach((hour) => {
    //   const utc = hour.dt * 1000 + localOffset;
    //   hour.currentTime =
    //     utc + 1000 * weatherData.data.timezone_offset;
    // });



    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
const weatherData = await getWeatherData();

    //extract timestemp
    const timestamp = weatherData.dt;
                 // Convert timestamp to a readable date and time
                  const date = new Date(weatherData.dt * 1000); // Convert from seconds to milliseconds
                  const  formattedDate = date.toLocaleDateString();
                  const  formattedTime = date.toLocaleTimeString();
</script>

