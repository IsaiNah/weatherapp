
<!-- v-movel below binds this field to the javascript variable above-->
<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <!-- v-model below binds this field to the JavaScript variable above -->
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      
      <!-- outputting li for each result available -->
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]" v-if="mapboxSearchResults">
        <p class="py-2" v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p class="py-2" v-if="!searchError && mapboxSearchResults.length === 0">No results match your query, try a different term.</p>
        <template v-else>
          <li v-for="searchResult in mapboxSearchResults" :key="searchResult.id" class="py-2 cursor-pointer" @click="previewCity(searchResult)">
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <!-- Because of the async getCities component with the await, we need to use Suspense -->
      <Suspense>
        <CityList />
        <template #fallback>
          <p>Loading...</p>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import CityList from "../components/CityList.vue";

const openWeatherAPIKey = '6334597926f131b0bc4fba671c654c0f';
const openWeatherSearchResult = ref(null); // Initialize as null
const searchQuery = ref('');
const searchError = ref(null);
let queryTimeout = null; // Use a variable to store the timeout reference

//Variables extracted from Query
let cityName = null;
let cityTemp = null;
let cityTempFeel = null;
let cityCountry = null;
let lat = null;
let log = null;

const router = useRouter();

//Getting variables from query and into new route
const previewCity = (cityname, citytemp, citytempfeel, cityCountry, lat, log)=>{
console.log('previewCity ', cityname, citytemp, citytempfeel);
// extracting city & other data 
//const city = searchResult.value.Name;
router.push({
  name: "cityView",
   params: {
     state: cityCountry, 
     city: cityname,
     tempfeel: citytempfeel,
     temp: citytemp, 
     
    },
    query:{
    lat: lat,
    log: log,
    preview:true,
    },
});
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
        //Below is openweatherapi call with geocoding so calling by name here https://openweathermap.org/current#data
        const result = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${searchQuery.value}&appid=${openWeatherAPIKey}&units=metric `
          
          );
        openWeatherSearchResult.value = result.data;
        
        console.log(openWeatherSearchResult.value);
        console.log('NAME IS : ', openWeatherSearchResult.value.name);
        console.log('TEMP IS : ', openWeatherSearchResult.value.main.temp);
        console.log('Feels TEMP IS : ', openWeatherSearchResult.value.main.feels_like);
        console.log('Sys . country : ', openWeatherSearchResult.value.sys.country);
        console.log('Lat :', openWeatherSearchResult.value.coord.lat);
        console.log('Lon :', openWeatherSearchResult.value.coord.lon);
        //TODO Now that I managed to extract data, I should pass it on to the next component 
        cityName = openWeatherSearchResult.value.name;
        cityTemp = openWeatherSearchResult.value.main.temp;
        cityTempFeel = openWeatherSearchResult.value.main.feels_like;
        cityCountry = openWeatherSearchResult.value.sys.country;
        lat = openWeatherSearchResult.value.coord.lat;
        log =  openWeatherSearchResult.value.coord.lon;


        console.log(cityName, ' ', cityTemp, ' ', cityTempFeel);

        previewCity(cityName, cityTemp, cityTempFeel, cityCountry, lat, log);
      
        //cityTemp = openWeatherSearchResult.value.
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