<template>
  <div class="flex flex-row justify-between py-5 px-8 bg-white boxshadow">
    <p class="text-[#111517] font-serif text-xl font-bold">
      Where in the world
    </p>
    <button class="text-[#111517] font-serif text-normal">Dark mode</button>
  </div>

  <div class="flex flex-row justify-between mt-10 py-5 px-8">
    <input
      type="text"
      placeholder="Search for the country..."
      class="w-[400px] boxshadow py-4 px-5 rounded-md"
    />
    <select name="" id="" class="boxshadow rounded-md p-2 pe-6 outline-none">
      <option class="text-black" value="Filter by region" selected>
        Filter by region
      </option>
      <option value="All">All</option>
      <option value="Africa">Africa</option>
      <option value="America">America</option>
      <option value="Asia">Asia</option>
      <option value="Europe">Europe</option>
      <option value="Oceania">Oceania</option>
    </select>
  </div>

  <div
    class="countries flex flex-row flex-wrap justify-between py-5 px-8"
    v-if="countriesData"
  >
    <div v-for="item in countriesData" :key="item">
      <div class="boxshadow w-[300px] mb-5">
        <img :src="item.flags.png" alt="" class="w-full rounded-lg" />
        <div class="pt-4 pl-6 pb-8">
          <h2 class="mb-5 text-[#111517] font-bold text-2xl">
            {{ item.name.common }}
          </h2>
          <p class="mb-3 text-[#111517] font-bold text-normal">
            Population: <span class="font-normal">{{ item.population }}</span>
          </p>
          <p class="mb-3 text-[#111517] font-bold text-normal">
            Region: <span class="font-normal">{{ item.region }}</span>
          </p>
          <p class="mb-3 text-[#111517] font-bold text-normal">
            Capital:
            <span class="font-normal">{{
              Array.isArray(item.capital)
                ? item.capital.join(", ")
                : item.capital
            }}</span>
          </p>
        </div>
      </div>
    </div>
  </div>
  <h3 v-else>loading</h3>
</template>

<style scoped>
.boxshadow {
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
}
</style>

<script setup>
import { ref, onMounted } from "vue";

let countriesData = ref(null); // Initialize countriesData as a ref

onMounted(async () => {
  try {
    const response = await fetch("https://restcountries.com/v3.1/all");
    const data = await response.json();
    countriesData.value = data; // Assign the data to the ref
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});
</script>
