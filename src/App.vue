<template>
  <div class="main">
    <div>
      <h1>América Latina: qual é a capital?</h1>
    </div>

    <div>
      <button @click="getRandomInt" v-show="!clicked">Começar</button>
    </div>

    <div class="main-space space"></div>

    <div class="board">
      <country-card v-show="clicked" :country="country.name" :capital="country.capital" />
    </div>
  </div>
</template>

<script>
import CountryCard from '@/components/CountryCard.vue'
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    CountryCard
  },

  data() {
    return {
      countries: [],
      countryNumbers: [...Array(15).keys()],
      country: '',
      randomInt: 0,
      points: 0,
      clicked: false,
    }
  },
  
  methods: {
    async fetchCountries() {
        const response = await fetch('https://restcountries.com/v2/all?fields=name,capital,subregion')
        this.countries = await response.json()
    },
    getRandomInt() {
      this.randomInt = _.sample(this.countryNumbers);
      this.countryNumbers.splice(this.countryNumbers.indexOf(this.randomInt), 1)
      this.country = this.filteredCountries[this.randomInt]
      this.clicked = true;
    }
  },

  beforeMount() {
    this.fetchCountries()
  },

  computed: {
    filteredCountries() {
      return this.countries.filter(country => country.subregion == 'South America');
    }
  }
}
</script>

<style>
body {
  background: white;
  background-image: radial-gradient(#c1c1c1 1px, transparent 0);
  background-size: 15px 15px;
  background-position: -19px -19px;
  overflow: hidden;
}

* {
  font-family: 'Source Sans Pro', sans-serif;
}

.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  height: 95vh;
}

h1 {
  display: inline-block;
  background-color: #e54f6d;
  padding: 0.5em;
  box-shadow: 20px 20px rgba(0,0,0,.15);
  margin-top: 1.5em;
  font-size: 1.5em;
}

button {
  padding: 1em;
  border: 0;
  box-shadow: 20px 20px rgba(0,0,0,.15);
  cursor: pointer;   
  margin-top: 1em;
  font-size: 0.9em;
}

.space {
  flex-grow: 1;
}

.board {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 3em;
}

@media (max-width: 600px)
{
  body {
    width: 100%;
    height: 100%;
    margin: 0;
    overflow: hidden;
    position: absolute;
    top: 0;
    left: 0;
  }

  .main {
    justify-content: start;
  }

  .main-space {
    display: none;
  }

  .board {
    margin-top: 5em;
  }
}
</style>
