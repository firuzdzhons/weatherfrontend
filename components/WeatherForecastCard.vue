<template>
    <v-card class="mt-2" max="800px">
        <v-card-text>
            <div class="d-flex justify-space-between" >
                <h2>{{weatherForecast.city.name}}</h2>
                <h3>{{weatherForecast.list[0].weather[0].main}}</h3>  
                <h2>{{weatherForecast.list[0].main.temp}} C</h2>
            </div>
        </v-card-text>
        <v-card-actions>
            <v-btn
                color="orange lighten-2"
                text
                @click="show = !show"
            >
                {{show ? 'Show less' : 'Show More'}}
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn
                icon
                @click="show = !show"
            >
                <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
            </v-btn>
        </v-card-actions>
        <v-expand-transition>
        <div v-show="show">
            <v-divider></v-divider>
            <v-card-text>
                <div class="d-flex justify-space-between"> 
                    <div class="text-center" v-for="item in weatherForecast.list">
                        <div class="mb-2">{{item.day}}</div>
                        <div>{{item.main.temp}} C</div>
                    </div>
                </div>
            </v-card-text>
        </div>
        </v-expand-transition>
    </v-card>
</template>
<script>
    export default {
        name: 'WeatherForecastCard',
        props: ['weatherForecast'],
        data() {
            return {
                show: false
            }
        }
    }
</script>