<template>
<div class="h-screen flex items-center justify-center bg-gradient-to-t bg-gray-500">
  <div class="bg-indigo-600 p-3 rounded-lg border-8 border-sky-500" v-if="error">
    <p class="text-white">No location matching {{queriedLocation}} was found.</p>
    <button @click="returnToHome()" class="my-1.5	text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Return to home</button>
  </div>
  <div v-if="!error" class="bg-indigo-600 p-3 rounded-lg">
    <div v-if="resultName && displayItems.length > 0">
    <div class="mb-2 font-bold text-white">Current weather for {{resultName}}</div>
    <b v-for="item in displayItems" :key="item">
      <div class="bg-gray-200 leading-10 h-10 w-96 rounded-sm mb-3 ">
        <p><i v-if="!item.img" class="ml-2" :class="item.icon"></i> {{item.name}} <font class="float-right mr-4">{{item.value}}{{item.unit}}</font></p>
      </div>
    </b>
    <button @click="returnToHome()" class="my-1.5	text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Return to home</button>
  </div>
  <div v-else>
    <input type="search" id="default-search" class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Search for a location..." required v-model="queriedLocation">
    <button @click="search()" class="my-1.5	text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Search</button>
  </div>
  </div>
</div>
</template>

<script>
// tset
export default {
  data() {
    return {
      error: false,
      queriedLocation: '',
      resultName: "",
      displayItems: []
    }
  },
  methods: {
    formatItem(name) {
      return name = name[0].toUpperCase() + name.substring(1);
    },
    async search() {
      if(this.queriedLocation.length > 0) {
        const response = await fetch(`https://api.weatherapi.com/v1/current.json?key=948af2c1958f455ab8e91750230202&q=${this.queriedLocation.toLowerCase()}`);
        const data = await response.json();
        if(data.error != undefined) {
          this.error = true;
        } else {
          this.resultName = data.location.name;
          this.displayItems.push({name: "Tempreature", value: data.current.temp_c, icon: "fa-solid fa-temperature-three-quarters", unit: "°C"});
          this.displayItems.push({name: "Country", value: data.location.country, icon: "fa-solid fa-globe"});
          this.displayItems.push({name: "Weather Condition", value: data.current.condition.text, icon: "fa-solid fa-cloud"});
          this.displayItems.push({name: "Wind Speed", value: data.current.wind_mph, icon: "fa-solid fa-wind", unit: "mph"})
        }
      }
    },
    returnToHome() {
      this.error = false, this.queriedLocation = "", this.displayItems = [];
    }
  }
}
</script>

<style>
input:focus,
select:focus,
textarea:focus,
button:focus {
    outline: none;
}
</style>