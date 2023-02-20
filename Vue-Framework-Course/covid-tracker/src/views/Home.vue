<template>
    <main v-if="!loading">
        <DataTitle :text="title" :dataDate="dataDate" />

        <DataBoxes :stats="stats" />

        <CountrySelect @get-country="getCountryData" :countries="countries" />

        <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white p-4 rounded mt-10 focus:outline-none hover:bg-green-500">
            Clear Selected
        </button>
    </main>

    <main class="flex flex-col align-center justify-center text-cent" v-else>
        <div class="text-gray-500 text-3xl mt-10 mb-6">
            Fetching Data...
        </div>
        <img :src="loadingImage" class="m-auto w-24" alt="Loading Gif">
    </main>
</template>

<script>
    import DataTitle from '@/components/DataTitle.vue'
    import DataBoxes from '@/components/DataBoxes.vue'
    import CountrySelect from '../components/CountrySelect.vue'

    import hourGlass from '../assets/hourglass.gif'
    export default {
        name: 'Home',
        components: {
            DataTitle,
            DataBoxes,
            CountrySelect
        },
        data() {
            return {
                loading: true,
                title: 'Global',
                dataDate: '',
                stats: {},
                countries: [],
                loadingImage: (hourGlass)
            }
        },
        methods: {
            async fetchCovidDate() {
                const res = await fetch('https://api.covid19api.com/summary')
                const data = await res.json()
                return data
                
            },
            getCountryData(country) {
                this.title = country.Country
                this.stats = country
            },
            async clearCountryData() {
                this.loading = true
                const data = await this.fetchCovidDate()
                this.title = 'Global'
                this.stats = data.Global
                this.loading = false
            }
        },
      async created() {
            const data = await this.fetchCovidDate()
            
            this.dataDate = data.Date
            this.stats = data.Global
            this.countries = data.Countries
            this.loading = false
        },
    }
</script>