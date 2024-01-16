<template>
   <header class="sticky top-2 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">

    <RouterLink :to="{name:'home'}">
        <div class="flex items-center gap-3">
        <i class="fa-solid fa-sun text-2xl"></i>
        <p class="text-2x1">
        The local Weather
        </p>
    </div>
</RouterLink>

    <div class="flex gap-3 flex-1 justify-end"> 
       <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
       @click="toggleModal"></i>
       <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
       @click="addCity"
       ></i>

    </div>

   
    </nav>
    
   </header>

   <BaseModal :modalActive="modalActive"  @close-modal="toggleModal">
    <div class="text-black">
          <h1 class="text-2xl mb-1">About:</h1>
          <p class="mb-4">
            The Local Weather allows you to track the current and
            future weather of cities of your choosing.
          </p>
          <h2 class="text-2xl">How it works:</h2>
          <ol class="list-decimal list-inside mb-4">
            <li>
              Search for your city by entering the name into the
              search bar.
            </li>
            <li>
              Select a city within the results, this will take
              you to the current weather for your selection.
            </li>
            <li>
              Track the city by clicking on the "+" icon in the
              top right. This will save the city to view at a
              later time on the home page.
            </li>
          </ol>

          <h2 class="text-2xl">Removing a city</h2>
          <p>
            If you no longer wish to track a city, simply select
            the city within the home page. At the bottom of the
            page, there will be am option to delete the city.
          </p>
        </div>
    </BaseModal>
</template>

<script setup>
import { ref } from 'vue';
import { uid } from 'uid';
import { useRoute, useRouter } from 'vue-router';
import { RouterLink } from 'vue-router';
import BaseModal from './BaseModal.vue';

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const modalActive = ref(null);

const toggleModal = () => {
    modalActive.value = !modalActive.value;
}

const addCity = () => {
    // Load existing cities from local storage
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(localStorage.getItem('savedCities'));
    }

    // Create a new city object
    const locationObj = {
        id: uid(),
        state: route.params.state,
        city: route.params.city,
        tempfeel: route.params.tempfeel,
        temp: route.params.temp,
    };

    // Push the new city object to the array
    savedCities.value.push(locationObj);

    // Save the updated array to local storage
    localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

    // Clear the preview query parameter
    let query = Object.assign({}, route.query);
    delete query.preview;
    router.replace({ query });
};
</script>
<!-- 
<script setup>
import { ref } from 'vue';
import { uid } from 'uid';
import { RouterLink } from 'vue-router';
import BaseModal from './BaseModal.vue';

const savedCities = ref([]);
const addCity = () => {
//   if (localStorage.getItem('savedCities')) {
//     savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
//   }
// };

const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
}

 //const route = userRoute();
// const addCity = () => {
//   if (localStorage.getItem('savedCities')) {
//     savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
//   }
// };
// 
// const locationObject = {
//   id: uid(),
//   state: route.params.state,
//   city: route;params.city,
//   coords: {
//     lat: route.query.lat,
//     lng: route.query.lng
//   }
// };
</script> -->
