<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-container>
        <v-row>
          <v-col cols="10">
            <v-autocomplete
              v-model="model"
              :items="items"
              :loading="isLoading"
              :search-input.sync="search"
              hide-selected
              item-text="fullname"
              item-value="fullname"
              label="Search"
              placeholder="Start typing to search"
              return-object
              outlined
              @change="searchWeatherForecast"
            ></v-autocomplete>
          </v-col>
          <v-col cols="2">
            <v-btn
              x-large
              @click="storeWeatherForecast"
              color="primary"
              :loading="isLoading"
              :disabled="isLoading || weatherForecasts.length == 0"
            >
              Save
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="10">
            <v-alert
                v-model="alert"
                dismissible
                color="primary"
                border="left"
                elevation="2"
                colored-border
                v-if="weatherForecastId"
              >
                <NuxtLink target="_blank" :to="'weather_forecast/'+weatherForecastId">Link to list</NuxtLink>
            </v-alert>
          </v-col>
        </v-row>
        <v-row>
          
          <v-col cols="10" v-for="weatherForecast in weatherForecasts" :key="weatherForecast.dt">
            <WeatherForecastCard :weatherForecast="weatherForecast"/>
          </v-col>
        </v-row>
      </v-container>
    </v-col>
  </v-row>
</template>
<script>
  export default {
    name: "IndexPage",
    data() {
        return {
            entries: [],
            isLoading: false,
            model: null,
            search: null,
            weatherForecasts: [],
            show: false,
            alert: true,
            weatherForecastId: '',
            timerId: ''
        };
    },
    computed: {
        items() {
          return this.entries.map(entry => {
              const fullname = entry.name + ", " + entry.country;
              return Object.assign({}, entry, { fullname });
          });
        },
    },
    watch: {
        search(val) {
          if (!val) return;

          this.searchCity(val)
        },
    },
    methods: {
      searchCity(city) {
        clearTimeout(this.timerId)

        this.isLoading = true;

        this._timerId = setTimeout(() => {
          this.$axios.$get(`city/search?name=${city}`)
          .then((res) => {
            this.entries = res
          })
          .catch(function (error) {
              console.log(error);
          }).finally(() => {
              this.isLoading = false;
          });
        }, 500)
        
      },
      searchWeatherForecast() {
        if(!this.model) return

        this.isLoading = true;
       
        this.$axios.$get(`weather-forecast/search?q=${this.model.fullname}`)
          .then((res) => {
            this.weatherForecasts.push(res);
          })
          .catch(err => {
            console.log(err);
          })
          .finally(() => (this.isLoading = false));
      },
      storeWeatherForecast() {
        this.isLoading = true;
        this.$axios.post('weather-forecast/store', {content: this.weatherForecasts})
          .then(({data}) => {
            this.weatherForecastId = data
          })
          .catch(function (error) {
            console.log(error);
          }).finally(() => {
            this.isLoading = false;
          });
      }
    }
}
</script>
