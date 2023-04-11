<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text"
      v-model="searchQuery"
      @input = "getSearchResults"
      placeholder="Search for City or Town"
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004e71]">
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      v-if="mapboxSearchResults">
      <p v-if="searchError"> Sorry Suck My Balls</p>
      <p v-if="!serverError && mapboxSearchResults.length === 0">No results match your query try a different term</p>
        <template v-else>
          <li
          v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer "
          @click="previewCity(searchResult)">
          {{ searchResult.place_name }}
                </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
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
import CityCardSkeleton from '../components/CityCardSkeleton.vue';

const router = useRouter();
const previewCity = (searchResult) =>{
  // console.log(searchResult);
  const [city , state] = searchResult.place_name.split(", ");
  router.push({
    name: "cityView",
    params: {state: state, city: city},
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
}

const mapboxAPIkey = "pk.eyJ1IjoibG9yZW56by1pYmF5IiwiYSI6ImNsZzBrZXh6ZjBzbmwzZW1ybjcyMHUwdTUifQ.-GDlzW2voRY5e0woGVTqgA";
const searchQuery = ref("") ;
const queryTimeOut = ref(null);
const mapboxSearchResults= ref(null);
const searchError = ref(null);
const getSearchResults = () => {
  clearTimeout(queryTimeOut.value);
 queryTimeOut.value = setTimeout( async()=>{
  try{
    if(searchQuery !== ""){
    const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIkey}&types=place`);
    mapboxSearchResults.value = result.data.features;
    return;
  }
  mapboxSearchResults.value = null;
  }catch{
    searchError.value = true;
  }
 },300)
}
</script>