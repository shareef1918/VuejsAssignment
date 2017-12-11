<template>
  <v-app>
    <v-toolbar dark color="primary">
      <v-toolbar-title class="white--text">{{title}}</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <main>
      <v-layout row>
        <v-flex xs12 sm6 offset-sm3>
          <v-card>
            <v-toolbar color="cyan" dark>
              <v-toolbar-title>Cities</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn color="error" dark @click.stop="dialog2 = true">Add New City</v-btn>              
            </v-toolbar>
            <v-list two-line v-if="cities.length>0">
              <template v-for="city in cities">
                <v-subheader v-if="city.header" v-text="city.header" :key="city.header"></v-subheader>
                <v-divider v-else-if="city.divider" v-bind:inset="city.inset" :key="city.divider"></v-divider>
                <v-list-tile avatar v-else v-bind:key="city.name" @click="getWeatherReport(city.name)">
                  <v-list-tile-avatar>
                    <img v-bind:src="city.avatar">
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title v-html="city.name"></v-list-tile-title>
                    <v-list-tile-sub-title v-html="city.description"></v-list-tile-sub-title>
                  </v-list-tile-content>
                </v-list-tile>
              </template>
            </v-list>
            <v-list v-else>
              <template>
                 <v-subheader> No Records Found </v-subheader>
              </template>
            </v-list>
          </v-card>
        </v-flex>
      </v-layout>
    </main>
    <v-dialog v-model="dialog2" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">Add New City</span>
        </v-card-title> 
        <v-card-text>
            <v-text-field
              label="Name"
              v-model="city"
              required
            ></v-text-field>
            <v-container fluid>
            <v-layout row>
              <v-flex xs12>
                <v-text-field
                  name="description"
                  v-model="description"
                  label="Description"
                  textarea
                ></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
          <v-card-actions>
            <v-btn color="primary" flat @click="submitForm">Submit</v-btn>
            <v-btn color="primary" flat @click.stop="dialog2=false">Close</v-btn>
          </v-card-actions>
        </v-card-text>
        </v-card>
      </v-dialog>
  </v-app>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data: () => ({
      title: 'List of Cities',
      dialog2: false,
      city: '',
      zipcode: '',
      description: '',
      items: [
        { header: 'Today' },
        { avatar: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSkcBxbbXmvedxZNwQcXRMyJqarZww0moK35f581zsx50D0TekFIw', title: 'Brunch this weekend?', subtitle: "<span class='grey--text text--darken-2'>Ali Connors</span> — I'll be in your neighborhood doing errands this weekend. Do you want to hang out?" },
        { divider: true, inset: true },
        { avatar: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSkcBxbbXmvedxZNwQcXRMyJqarZww0moK35f581zsx50D0TekFIw', title: 'Summer BBQ <span class="grey--text text--lighten-1">4</span>', subtitle: "<span class='grey--text text--darken-2'>to Alex, Scott, Jennifer</span> — Wish I could come, but I'm out of town this weekend." },
        { divider: true, inset: true },
        { avatar: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSkcBxbbXmvedxZNwQcXRMyJqarZww0moK35f581zsx50D0TekFIw', title: 'Oui oui', subtitle: "<span class='grey--text text--darken-2'>Sandra Adams</span> — Do you have Paris recommendations? Have you ever been?" },
      ],
    }),
    computed: {
      cities() {
        return this.$store.state.cities;
      },
    },
    methods: {
      submitForm() {
        this.$store.dispatch('ADD_CITY', { name: this.city, description: this.description });
        this.city = '';
        this.description = '';
        this.dialog2 = false;
      },
      getWeatherReport(cityName) {
        this.$router.push({
          name: 'CityWeather',
          params: { cityName },
        });
      },
    },
  };
</script>
<style>
.card{
  margin-top:30px;
}
</style>