
<!-- v-movel below binds this field to the javascript variable above-->
<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" 
      v-model="searchQuery" 
      @input="getSearchResults"
      placeholder="Search for a city or state" 
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary
      focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"/>
      
       <!-- outputting li  for each result avaliable -->
      <!-- <ul class="absolute bg-weather-secondary text-white
      w-full shadow-md py2 px-1 top-[66px]">.

    
     <li v-for="searchResult in openWeatherSearchResult" 
     :key="searchResult.id"
     class="py-2 cursor-pointer" 
     >
    
     Temperature: {{ searchResult.main && searchResult.main.temp }}°C,
            Feels Like: {{ searchResult.main && searchResult.main.feels_like }}°C,
            Wind Speed: {{ searchResult.wind && searchResult.wind.speed }} m/s
    </li>
   
    </ul> -->

    <!-- outputting li for each result available -->
<!-- <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
  <li v-for="searchResult in openWeatherSearchResult" :key="searchResult.id" class="py-2 cursor-pointer">
    <p>
      <span class="font-bold">{{ searchResult.name }}</span>
      - Temperature:   {{ seacrhResult.temp}}
    </p>
  </li>
</ul> -->


<!-- <ul v-if="searchResults" class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
        <li v-for="searchResult in openWeatherSearchResult" :key="searchResult.id" class="py-2 cursor-pointer">
          <p v-if="searchResult.main && searchResult.wind">
            <span class="font-bold">{{ result.name }}</span> -
            Temperature: {{ searchResult.temp || 'N/A' }}°C,
            Feels Like: {{ searchResult.main.feels_like || 'N/A' }}°C,
            Wind Speed: {{ searchResult.wind.speed || 'N/A' }} m/s
          </p>
          <p v-else>
            No weather data available for {{ searchResult.name }}
          </p>
        </li>
      </ul> -->

   
    </div>
    
     </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
console.log("Console Working");
const openWeatherAPIKey = '6334597926f131b0bc4fba671c654c0f';
const openWeatherSearchResult = ref(null); // Initialize as null
const searchQuery = ref('');
let queryTimeout = null; // Use a variable to store the timeout reference

// Getting search results using a lazy search (user types in input and we start loading soon after typing stops)
const getSearchResults = async () => {
  console.log('getSearchResults called');
  // This clears the previous setTimeout when the user stops writing; the searchQuery is run after 300 milliseconds
  clearTimeout(queryTimeout);

  // Executes the following code after setTimeout reaches 0
  queryTimeout = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&appid=${openWeatherAPIKey}&type=name`
          //REMOVE THIS check gpt for updated code
          );
        openWeatherSearchResult.value = result.data;
        console.log(openWeatherSearchResult.value);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    } else {
      // Below is only needed IN CASE the value in the search is empty; otherwise, we return above
      // Return is there to prevent setting the value to null otherwise
      openWeatherSearchResult.value = null;
      console.error('openWeatherSearchResult set to null');
    }
  }, 300);
};
</script>