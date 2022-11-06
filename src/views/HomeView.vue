<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResult"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        v-if="apiSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      >
        <li
          v-for="searResult in apiSearchResults"
          :key="searResult.id"
          class="py-2 cursor-pointer"
        >
          {{ searResult.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const myKey = import.meta.env.VITE_MAPBOX_KEY;

const searchQuery = ref('');
const queryTimeout = ref(null);
const apiSearchResults = ref(null);
const getSearchResult = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${myKey}&types=place`
      );
      apiSearchResults.value = result.data.features;
      console.log(apiSearchResults.value);
      return;
    }
    apiSearchResults.value = null;
  }, 300);
};
</script>
