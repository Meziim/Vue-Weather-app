<template>
  <div class="container-fluid">
    <header class="row justify-content-between py-3 px-lg-5 bg-light text-dark">
      <a href="/" class="col-5 col-lg-7 d-flex align-items-center justify-content-start text-dark text-decoration-none">
        <span class="fs-3 appLogo">Weather App</span>
      </a>
      <div id="input-group" class="col align-items-center input-group">
        <input 
          class="form-control" 
          v-model="searchInput" type="text"
          placeholder="Location" 
          @blur="closeSuggBox"
          @input="fetchLocations"
          @keyup="selectedItem"
        />
        <Suggestions 
          @locationClick="fetchCurrent" 
          v-if="searchResults !== null" 
          v-bind:list="searchResults" 
          v-bind:selectedItem="listItemNum"
          @componentDismount="listItemNum = null"
          @hoverItem="setSelectedItem"
        />
        <div class="input-group-append">
          <button @click="fetchCurrent(searchInput)" class="btn btn-primary" type="button">Search</button>
        </div>
      </div>
    </header>
    <div v-if="fetchfail" class="error m-5 bg-light p-4 rounded">
      <h1 class="text-danger fs-5">Failed to fetch data from server, <a href="#" class="text-dark" @click="fetchCurrent(searchInput)">Retry</a>.</h1>
    </div>
    <div v-if="spinnerLoading" class="loading-screen row d-flex justify-content-center m-5">
      <div class="spinner-border text-light" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
    <HeroCard v-if="heroCreated" :data="weatherData" :isMetric="countryUnit" />
    <div class="mx-lg-5">
      <WeekData
        v-if="heroCreated" 
        :forecastDays="weatherData.forecast.forecastday"
      />
      <WeatherMap />
    </div>

    
  </div>
</template>

<script>
import HeroCard from './components/HeroCard';
import Suggestions from './components/SgList';
import WeekData from './components/weekData';
import WeatherMap from './components/weatherMap';



export default {
  name: 'App',
  components: {
    HeroCard,
    Suggestions,
    WeekData,
    WeatherMap,
  },
  data() {
    return {
      searchInput: null,

      searchResults: null,
      countryUnit: false,
      weatherData: null,

      heroCreated: false,
      fetchfail: false,
      spinnerLoading: true,
      listItemNum: null,
    }
  },
  methods: {
    async fetchLocations() {
      if(this.searchInput.length >= 3) {
        

        const req = await fetch(`https://api.weatherapi.com/v1/search.json?key=${process.env.VUE_APP_API_KEY}&q=${this.searchInput}`);

        let searchApi = await req.json();
        this.searchResults = searchApi.slice(0, 5);
        
      } else {
        this.searchResults = null;
      }
    },
    async fetchCurrent(url) {
      this.heroCreated = false;
      this.spinnerLoading = true;
      this.fetchfail = false;
      let failedFetch = setTimeout(() => {
        this.fetchfail = true;
        this.spinnerLoading = false;
        console.log('error fetching data from API');
        return
      }, 10000)
      function json(url) {
        return fetch(url).then(res => res.json());
      }
      if(!this.fetchfail) {

        this.weatherData = await json(`http://api.weatherapi.com/v1/forecast.json?key=${process.env.VUE_APP_API_KEY}&q=${url}&days=5&aqi=yes&alerts=no`);
        console.log(this.weatherData);


        this.searchInput = `${this.weatherData.location.name}, ${this.weatherData.location.country}`;


        //Checking if country of the data is a country that uses IMPERIAL system
        if(this.weatherData.location.country === "Liberia" | this.weatherData.location.country === "United States of America" | this.weatherData.location.country === "Myanmar") {
          this.countryUnit = false;
        } else {
          this.countryUnit = true;
        }


        clearTimeout(failedFetch)
        this.spinnerLoading = false;
        this.heroCreated = true;
      }
    },




    closeSuggBox() {
      setTimeout(() => {
        //this delays the dismounting of the suggestion list component, to allow time for click event to be registered.
        this.searchResults = null;
      }, 300);
    },



    setSelectedItem(num) {
      this.listItemNum = num;
    },
    selectedItem(e) {
      if(e.keyCode === 38) {
        switch (this.listItemNum) {
          case null:
            this.listItemNum = 4
            break;
          case 0:
            this.listItemNum = 4;
            break;
          default:
            this.listItemNum--
            break;
        }
      } else if(e.keyCode === 40) {
        switch (this.listItemNum) {
          case null:
            this.listItemNum = 0
            break;
          case 4:
            this.listItemNum = 0
            break;
          default:
            this.listItemNum++
            break;
        }
      } else if(e.keyCode === 13) {
        if (this.listItemNum === null && this.searchResults === null) {
          this.fetchCurrent(this.searchInput);
        } else if (this.listItemNum === null && this.searchResults !== null) {
          this.fetchCurrent(this.searchResults[0].url)
          this.searchResults = null;
        } else {
          this.fetchCurrent(this.searchResults[this.listItemNum].url);
          this.searchResults = null;
        }
      }
    }
  },
  created: async function() {
    function json(url) {
      return fetch(url).then(res => res.json());
    }
    const ipdata = await json(`https://api.ipdata.co?api-key=${process.env.VUE_APP_IP_API}`);

    await this.fetchCurrent(ipdata.country_name);

    this.heroCreated = true;
  }
}






</script>

<style>

body {
  background: linear-gradient(to right, #667db6, #0082c8, #0082c8, #667db6); 

}
#weathericon {
  height: 80px;
  transform: scale(150%);
}
</style>
