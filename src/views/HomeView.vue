
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
      

    <!-- outputting li for each result available -->
<ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
 <p v-if="searchError"> City could not be found on this planet </p>
 
 <template v-else>
   <li v-for="searchResult in openWeatherSearchResult" 
   :key="searchResult.id" 
   class="py-2 cursor-pointer"
   
   >
      <p>
        <span class="font-bold">{{ searchResult }}</span>
         {{ seacrhResult}}
      </p>
    </li>
 </template>
</ul>


   
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
const searchError = ref(null);
let queryTimeout = null; // Use a variable to store the timeout reference

const previewCity = (searchResult)=>{
console.log('previewCity ', searchResult);
// extracting city & other data 
const city = searchResult.value.Name;
console.log('City:  ', city);
};

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
          `https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&appid=${openWeatherAPIKey}`
          
          );
        openWeatherSearchResult.value = result.data;
        previewCity(openWeatherSearchResult) 
        console.log(openWeatherSearchResult.value);
        console.log('NAME IS : ', openWeatherSearchResult.value.name);
      } catch (error) {
        console.error('Error fetching data:', error);
        searchError.value = true;
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