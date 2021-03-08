<template>
  <div class="App" :class="{ darkTheme : isDarkTheme }">
    <h1>Hello</h1>
    <div class="searchbar">
      <div class="searchContainer">
        <i class="fas fa-search searchIcon"> </i>
          <input
            class="searchInput"
            type="text"
            v-model="search"
            aria-label="Search for a country..."
            placeholder="Search for a country..."
          />
          <ul class="searchResults"></ul>
      </div>
      <div class="dropdownDiv">
        <a
          class="dropdownBtn"
          v-if="!showAllRegion"
          v-on:click="showFilter = !showFilter">
        </a>
        <ul v-if="showFilter" class="dropdownUL">
          <li>
            <label for="radioAfrica">Africa
              <input
                id="radioAfrica"
                class="dropdownInput"
                type="radio"
                name="africa"
                value="Africa"
                v-model="region"
                v-on:click="handleFilterClick"
              />
            </label>
          </li>
          <li>
            <input
              id="radioAmerica"
              class="dropdownInput"
              type="radio"
              name="america"
              value="America"
              v-model="region"
              v-on:click="handleFilterClick"
            />      
            <label for="radioAmerica">America</label>
          </li>
          <li>
            <input
              id="radioAmerica"
              class="dropdownInput"
              type="radio"
              name="asia"
              value="Asia"
              v-model="region"
              v-on:click="handleFilterClick"
            />      
            <label for="radioAmerica">Asia</label>
          </li>
          <li>
            <input
              id="radioEurope"
              class="dropdownInput"
              type="radio"
              name="europe"
              value="Europe"
              v-model="region"
              v-on:click="handleFilterClick"
            />      
            <label for="radioAmerica">Europe</label>
          </li>
          <li>
            <input
              id="radioOceania"
              class="dropdownInput"
              type="radio"
              name="oceania"
              value="Oceania"
              v-model="region"
              v-on:click="handleFilterClick"
            />      
            <label for="radioAmerica">Oceania</label>
          </li>
          <li>
            <input
              id="radioAll"
              class="dropdownInput"
              type="radio"
              name="all"
              value="All Regions"
              v-model="region"
              v-on:click="handleFilterClick"
            />      
            <label for="radioAmerica">All Regions</label>
          </li>
        </ul>
      </div>
    </div>
  
  <h1 v-if="error !== null">Sorry an error has occurred</h1>
  <div class="loaderFlex"><div v-if="pending" class="loader"></div></div>

  <div v-if="countryData" class="titleGrid">
    <div v-for="country in filteredCountries" class="countryTitle" v-bind:key="country.id">
      <router-link
        :to="{ name: 'Countries', params: {country: country.name}}"
        class="linkTitle"
      >
        <img v-bind:src="country.flag" alt="Country Flag" class="flag">
        <div class="text">
          <h1>{{ country.name}}</h1>
          <p><span>Population:</span>{{country.population}}</p>
          <p><span>Region:</span>{{country.region}}</p>
          <p><span>Capital:</span>{{country.capital}}</p>
        </div>
      </router-link>
    </div>
  </div>
</div>
</template>
  
<script>
  import axios from 'axios';
  
  export default {
    name: 'App',
    props: [ 'isDarkTheme'],
  
    data () {
      return {
        pending: false,
        error: null,
        countryData: null,
        search: '',
        showFilter: false,
        showAllRegion: false,
        region: '',
        darkMode: false,
    }
  },
  mounted () {
    this.pending = true;
    axios
        .get('https://restcountries.eu/rest/v2/all')
        .then(response => (this.countryData = response.data))
        .catch(error => (this.error = error ))
        .finally( () => { this.pending = false})
  },
  computed: {
    filteredCountries: function () {
        return this.countryData.filter((country) => {
            if (this.region === ''  || this.region === 'All Regions') {
                return country.name.match(this.search);
            }   else if (this.search !== '') {
                return country.name.match(this.search);
            }   else {
                return country.region.match(this.region);
            }
        })
    }
  },
  methods: {
    handleFilterClick () {
        setTimeout(() => {
            this.showFilter = !this.showFilter;
            this.showAllRegion = true;
        })       
    },
    formatNumbers (value) {
      return `${value.toLocaleString()}`
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
