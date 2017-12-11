<template>
  <v-layout column>
    <v-flex xs12 sm6 offset-sm3>
      <v-toolbar color="indigo" dark>
        <v-toolbar-title>{{cityDetails.name}}123</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn icon @click="updateView()" v-show="flex==12">
          <v-icon large color="white darken-2">dialpad</v-icon>
        </v-btn>
        <v-btn icon @click="updateView()" v-show="flex==6">
          <v-icon large color="white darken-2">list</v-icon>
        </v-btn>
      </v-toolbar>
      <v-container fluid grid-list-md class="grey lighten-4">
        <v-layout row wrap v-if="reports.length>0">
          <v-flex
            v-bind="{ [`xs${flex}`]: true }"
            v-for="(report,index) in reports"
            :key="report.dt"
          >
            <v-card>
              <v-card-media
                v-bind:src="src[index]"
                height="15  0px"
              >
                <v-container fill-height fluid>
                  <v-layout fill-height>
                    <v-flex xs12 align-end flexbox>
                      <span class="headline white--text">{{report.dt_txt | dateFormat}}</span>
                    </v-flex>
                    <v-flex xs12 align-end flexbox>
                      <span class="headline white--text">{{report.main.temp}}&deg; C</span>
                    </v-flex>
                    <v-flex xs12 align-end flexbox>
                    <v-card-media
                      v-bind:src="report.img"
                      height="100px" width="50px"
                      >
                    </v-card-media>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-card-media>
              <v-card-actions class="white">
                
              </v-card-actions>
            </v-card>
          </v-flex>
        </v-layout>
        <v-layout v-else>
          <v-card-actions class="white">
            No Records Found.
          </v-card-actions>
        </v-layout>
      </v-container>
    </v-flex>
  </v-layout>
</template>

<script>
  const axios = require('axios');

  export default {
    name: 'CityWeather',
    data: () => ({
      size: 'sm',
      reports: [],
      flex: 12,
      src: [
        'https://cdn.wallpapersafari.com/60/47/zltocg.jpg',
        'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSazIKmRV3I8XPcBdoTXwy26BE9EtBhw4fvisFCg0JG4z7KtJfU',
        'https://images.freecreatives.com/wp-content/uploads/2016/04/Download-Solid-Color-Background.jpg',
        'http://allpicts.in/wp-content/uploads/2017/08/Mustard-Color-Wallpaper-for-Solid-Background-500x281.jpg',
        'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTIubQdwgbsfDFKK-tAWiGEzPyfiQtkSd45rXP1RAfsDIkyGSS6'],
      cityDetails: {},
      cards: [
        { title: 'Pre-fab homes', src: '/static/doc-images/cards/house.jpg' },
        { title: 'Favorite road trips', src: '/static/doc-images/cards/road.jpg' },
        { title: 'Best airlines', src: '/static/doc-images/cards/plane.jpg' },
      ],
    }),
    created() {
      this.fetchData();
    },
    methods: {
      fetchData() {
        const city = this.$route.params.cityName;
        const cityName = city.charAt(0).toUpperCase() + city.slice(1).toLowerCase();
        axios.get('http://api.openweathermap.org/data/2.5/forecast?q=' + cityName + ',IN&units=metric&appId=0588189ab51e44f65305932d6755e701')
        .then((response) => {
          if (response) {
            for (let i = 0; i < response.data.list.length; i += 8) {
              if (response.data.list[i].weather[0].main === 'Clouds') {
                response.data.list[i].img = '/static/images/cloudiness.png';
              } else if (response.data.list[i].weather[0].main === 'Clear') {
                response.data.list[i].img = '/static/images/sunny.png';
              } else if (response.data.list[i].weather[0].main === 'Rain') {
                response.data.list[i].img = '/static/images/rain.png';
              } else if (response.data.list[i].weather[0].main === 'Snow') {
                response.data.list[i].img = '/static/images/snow.png';
              }
              this.reports.push(response.data.list[i]);
            }
            this.cityDetails = response.data.city;
          }
        }).catch((error) => {
          this.reports = [];
          this.cityDetails.name = cityName;
        });
      },
      updateView() {
        if (this.flex === 6) {
          this.flex = 12;
        } else {
          this.flex = 6;
        }
      },
    },
  };
</script>
<style>
.toolbar__title{
  color:#fff !important;
  font-weight:bold;
}
</style>