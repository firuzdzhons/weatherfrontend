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
              hide-no-data
              hide-selected
              item-text="fullname"
              item-value="fullname"
              label="Public APIs"
              placeholder="Start typing to Search"
              prepend-icon="mdi-database-search"
              return-object
              @change="getWeather"
            ></v-autocomplete>
          </v-col>
          <v-col cols="2">
            <v-btn
              color="primary"
            >
              Save
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
       <v-container>
          <v-row>
            <v-col cols="10" v-for="w in weather">
              <v-card class="mt-2">
                  <v-card-text>
                    <div class="d-flex justify-space-between" >
                      <h2>{{w.name}}</h2>
                      <h3>{{w.weather[0].main}}</h3>  
                      <h2>{{w.main.temp}} C</h2>
                    </div>
                  </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      nameLimit: 60,
      entries: [],
      isLoading: false,
      model: null,
      search: null,
      weather: []
    }
  },
  computed: {
    items () {
        return this.entries.map(entry => {
          // const name = entry.name.length > this.nameLimit
          //   ? entry.name.slice(0, this.nameLimit) + '...'
          //   : entry.name
          const fullname = entry.name+', '+entry.country

          return Object.assign({}, entry, { fullname })
        })
      }
  },
  watch: {
    search (val) {
      // console.log(val)
      // // Items have already been loaded
      // if (this.items.length > 0) return

      // this.items = []
      // Items have already been requested
      if (this.isLoading) return

      this.isLoading = true

      // Lazily load input items
      fetch(`http://weatherapi/api/find-city?name=${val}`)
        .then(res => res.json())
        .then(res => {
          // const { count, entries } = res
          // console.log(entries)
          // console.log(res)
          this.count = res.length
          this.entries = res
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    },
  },
  methods: {
    getWeather()
    {
       fetch(`http://weatherapi/api/get-weather-data?q=${this.model.fullname}`)
        .then(res => res.json())
        .then(res => {
          // const { count, entries } = res
          // console.log(entries)
          // console.log(res)
          // this.count = res.length
          // this.entries = res
          this.weather.push(res)
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    }
  },
}
</script>
