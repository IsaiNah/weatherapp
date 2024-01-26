<template>
    <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" @click="goToCityView(city)"/>
    </div>

    <p v-if="savedCities.length === 0">
    No locations added. To start tracking a location, search in
    the field above.
  </p>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRoute, useRouter } from 'vue-router';
import CityCard from "./CityCard.vue";

const openWeatherAPIKey = '6334597926f131b0bc4fba671c654c0f';

const savedCities = ref([]);

const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(localStorage.getItem("savedCities"));

        const requests = [];

        savedCities.value.forEach((city) => {
            console.log('THIS IS SAVEDCITIES LOOP', city.city)
            requests.push(
                axios.get(
                    `https://api.openweathermap.org/data/2.5/weather?q=${city.city}&appid=${openWeatherAPIKey}&units=metric`
                )
            );
        });
        const weatherData = await Promise.all(requests);

        weatherData.forEach((value, index) => {
            savedCities.value[index].weather = value.data;
        }); 
    }
};

await getCities();

const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: "cityView",
        params: {
            state: city.state, 
            city: city.city,
            tempfeel: city.tempfeel,
            temp: city.temp},
        query:  {id: city.id},
    });
};
</script>


