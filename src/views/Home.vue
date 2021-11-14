<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>
    <CountrySelect :countries="countries" @get-country="getCountryData"/>

    <button v-if="stats.Country" @click="clearCountryData" 
    class="bg-indigo-700 font-bold text-white rounded p-3 mt-10 focus:outline-none hover:bg-indigo-500
    transition duration-500 ease-in-out transform hover:-translate-y-1 hover:scale-110 hover:text-black">Clear Country</button>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script>
import DataTitle from '../components/DataTitle.vue'
import DataBoxes from '../components/DataBoxes.vue'
import CountrySelect from '../components/CountrySelect.vue'

export default {
  name: 'Home',
  components: { DataTitle,DataBoxes,CountrySelect},
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/logo.png')
    }
  },
  methods: {
   async fetchCovidData() {
     const request = await fetch('https://api.covid19api.com/summary')
     const data = await request.json()
     return data
    },
    getCountryData(country) {
      this.stats = country
      this.title = country.Country
    },
   async clearCountryData() {
     this.loading = true
    const data  = await this.fetchCovidData()
    this.title = 'Global'
    this.stats = data.Global
    this.loading = false
    }
  },
 async created() {
    const data =  await this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false    
  }
}
</script>
