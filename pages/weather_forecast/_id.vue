s (46 sloc)  1.29 KB

<template lang="">
    <v-row justify="center" align="center">
        <v-col cols="8" v-for="weatherForecast in weatherForecasts" :key="weatherForecast.dt">
           <WeatherForecastCard v-if="weatherForecasts" :weatherForecast = "weatherForecast"/>
        </v-col>
    </v-row>
</template>
<script>
export default {
    name: 'ShowWeatherForecast',
    data() {
        return {
            weatherForecasts: []
        }
    },
    mounted() {
        this.fetchAdvtById(this.$route.params.id);
    },
    methods: {
        fetchAdvtById(id) {
            this.$axios.get(`weather-forecast/show/${id}`)
                .then(({data}) => {
                    this.weatherForecasts = data.content
                })
                .catch(function (error) {
                    console.log(error);
                })
        }
    },
}
</script>
<style lang="">
    
</style>