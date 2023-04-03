<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text" 
      v-model="searchQuery"
      @input = "getSearchResults"
      placeholder="Search for City or State" 
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004e71]">
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      v-if="mapboxSearchResults">
        <li 
        v-for="searchResult in mapboxSearchResults"
        :key="searchResult.id"
        class="py-2 cursor-pointer ">
        {{ searchResult.place_name }}
      </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const mapboxAPIkey = "pk.eyJ1IjoibG9yZW56by1pYmF5IiwiYSI6ImNsZzBrZXh6ZjBzbmwzZW1ybjcyMHUwdTUifQ.-GDlzW2voRY5e0woGVTqgA";
const searchQuery = ref("") ;
const queryTimeOut = ref(null);
const mapboxSearchResults= ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value);
 queryTimeOut.value = setTimeout( async()=>{
  if(searchQuery !== ""){
    const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIkey}&types=place`);
    mapboxSearchResults.value = result.data.features;
    return;
  }
  mapboxSearchResults.value = null;
 },300)
}
</script>