<template>
  <div class="dark:bg-[#202c37]">
    <div
      class="flex flex-row justify-between py-5 sm:px-8 px-4 bg-white boxshadow dark:bg-[#2b3945]"
    >
      <a
        href="/"
        class="text-[#111517] font-serif sm:text-xl text-normal font-bold cursor-pointer dark:text-white"
      >
        Where in the world
      </a>
      <button
        class="text-[#111517] font-serif font-bold text-normal flex flex-row items-center"
        v-if="!modeValues"
        @click="toggleMode"
      >
        <img
          src="../images/moon.svg"
          alt="image"
          class="w-5 h-5 sm:mr-3 mr-1"
        />
        Dark mode
      </button>

      <button
        class="text-[#111517] font-serif font-bold text-normal flex flex-row items-center dark:text-white"
        v-if="modeValues"
        @click="toggleMode"
      >
        <img src="../images/sun.svg" alt="image" class="w-5 h-5 mr-3" />
        Light mode
      </button>
    </div>

    <div
      class="flex flex-col sm:flex-row sm:justify-between justify-center mt-10 py-5 px-8"
    >
      <input
        type="text"
        placeholder="Search for the country..."
        v-model="searchInput"
        @keyup="searchData"
        class="search-input max-w-[400px] boxshadow py-4 px-5 rounded-md focus:invalid outline-none mb-3 md:mb-0 dark:bg-[#2b3945] dark:text-white dark:placeholder-white"
      />
      <select
        name=""
        id=""
        v-model="selectedValue"
        @change="filterData"
        class="boxshadow rounded-md p-2 pe-6 outline-none max-w-[400px] dark:bg-[#2B3945] dark:text-white"
      >
        <option value="Filter by region" selected>Filter by region</option>
        <!-- <option value="All">All</option> -->
        <option value="Africa">Africa</option>
        <option value="America">America</option>
        <option value="Asia">Asia</option>
        <option value="Europe">Europe</option>
        <option value="Oceania">Oceania</option>
      </select>
    </div>

    <div
      class="countries flex flex-row flex-wrap sm:justify-between justify-center py-5 px-8"
      v-if="countriesData"
    >
      <div v-for="item in countriesData" :key="item">
        <div
          class="boxshadow w-[300px] mb-8 cursor-pointer hover:scale-110 duration-300 sm:items-center dark:bg-[#2B3945] dark:rounded-t-lg"
        >
          <img
            :src="item.flags.png ? item.flags.png : item.flags.svg"
            alt=""
            class="w-full rounded-lg dark:border-[#2b3945]"
          />
          <div class="pt-4 pl-6 pb-8">
            <h2 class="mb-5 text-[#111517] font-bold text-2xl dark:text-white">
              {{ item.name.common }}
            </h2>
            <p
              class="mb-3 text-[#111517] font-bold text-normal dark:text-white"
            >
              Population: <span class="font-normal">{{ item.population }}</span>
            </p>
            <p
              class="mb-3 text-[#111517] font-bold text-normal dark:text-white"
            >
              Region: <span class="font-normal">{{ item.region }}</span>
            </p>
            <p
              class="mb-3 text-[#111517] font-bold text-normal dark:text-white"
            >
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
    <div v-if="!countriesData" class="loading">Loading&#8230;</div>
  </div>
</template>

<style scoped>
.boxshadow {
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.08);
}
/* Absolute Center Spinner */
.loading {
  position: fixed;
  z-index: 999;
  height: 1em;
  width: 2em;
  overflow: show;
  margin: auto;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}

/* Transparent Overlay */
.loading:before {
  content: "";
  display: block;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.3);
}

/* :not(:required) hides these rules from IE9 and below */
.loading:not(:required) {
  /* hide "loading..." text */
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}

.loading:not(:required):after {
  content: "";
  display: block;
  font-size: 10px;
  width: 1em;
  height: 1em;
  margin-top: -0.5em;
  -webkit-animation: spinner 1500ms infinite linear;
  -moz-animation: spinner 1500ms infinite linear;
  -ms-animation: spinner 1500ms infinite linear;
  -o-animation: spinner 1500ms infinite linear;
  animation: spinner 1500ms infinite linear;
  border-radius: 0.5em;
  -webkit-box-shadow: rgba(0, 0, 0, 0.75) 1.5em 0 0 0,
    rgba(0, 0, 0, 0.75) 1.1em 1.1em 0 0, rgba(0, 0, 0, 0.75) 0 1.5em 0 0,
    rgba(0, 0, 0, 0.75) -1.1em 1.1em 0 0, rgba(0, 0, 0, 0.5) -1.5em 0 0 0,
    rgba(0, 0, 0, 0.5) -1.1em -1.1em 0 0, rgba(0, 0, 0, 0.75) 0 -1.5em 0 0,
    rgba(0, 0, 0, 0.75) 1.1em -1.1em 0 0;
  box-shadow: rgba(0, 0, 0, 0.75) 1.5em 0 0 0,
    rgba(0, 0, 0, 0.75) 1.1em 1.1em 0 0, rgba(0, 0, 0, 0.75) 0 1.5em 0 0,
    rgba(0, 0, 0, 0.75) -1.1em 1.1em 0 0, rgba(0, 0, 0, 0.75) -1.5em 0 0 0,
    rgba(0, 0, 0, 0.75) -1.1em -1.1em 0 0, rgba(0, 0, 0, 0.75) 0 -1.5em 0 0,
    rgba(0, 0, 0, 0.75) 1.1em -1.1em 0 0;
}

/* Animation */

@-webkit-keyframes spinner {
  0% {
    -webkit-transform: rotate(0deg);
    -moz-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@-moz-keyframes spinner {
  0% {
    -webkit-transform: rotate(0deg);
    -moz-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@-o-keyframes spinner {
  0% {
    -webkit-transform: rotate(0deg);
    -moz-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes spinner {
  0% {
    -webkit-transform: rotate(0deg);
    -moz-transform: rotate(0deg);
    -ms-transform: rotate(0deg);
    -o-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    -moz-transform: rotate(360deg);
    -ms-transform: rotate(360deg);
    -o-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

.search-input:before {
  content: "";
  display: block;
  background-image: url("../images/search.svg");
  position: absolute;
}
</style>

<script setup>
import { ref, onMounted, watch } from "vue";

let countriesData = ref(null); // Initialize countriesData as a ref
let modeValues = ref(true);
let selectedValue = ref("Filter by region");
let searchInput = ref("");

const fetchData = async () => {
  try {
    const response = await fetch(`https://restcountries.com/v3.1/all`);
    const data = await response.json();
    countriesData.value = data;
  } catch (error) {
    console.error("An error occurred:", error);
  }
};

const filterData = async () => {
  try {
    countriesData.value = null;
    const response = await fetch(
      `https://restcountries.com/v3.1/region/${selectedValue.value}`
    );
    const data = await response.json();
    countriesData.value = data;
  } catch (error) {
    console.error("An error occurred:", error);
  }
};

const searchData = async () => {
  try {
    if (searchInput.value.length === 0) {
      fetchData();
      return;
    }
    const response = await fetch(
      `https://restcountries.com/v3.1/name/${searchInput.value}`
    );
    // countriesData.value = null;
    const data = await response.json();
    countriesData.value = data;
  } catch (error) {
    console.error("An error occurred:", error);
  }
};
watch(selectedValue, filterData);
watch(searchInput, searchData);

onMounted(fetchData);
function toggleMode() {
  console.log(modeValues.value);
  modeValues.value = !modeValues.value;

  if (modeValues.value) {
    document.body.classList.add("dark");
  } else {
    document.body.classList.remove("dark");
  }
}
</script>
