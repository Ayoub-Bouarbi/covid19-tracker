<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="4" md="4">
        <p class="my-1 border-l-4 p-2 text-gray-700 text-left border-red-500">Today Overview</p>
      </v-col>
      <v-col cols="12" sm="4" md="4">
        <p class="my-1 text-gray-700 text-center text-2xl">{{ country }}</p>
      </v-col>
      <v-col cols="12" sm="4" md="4">
        <p class="my-1 text-right text-gray-500" v-text="(new Date()).toDateString()"></p>
      </v-col>
    </v-row>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="red--text my-1 darken-4">Confirmed</p>
          <p class="red--text darken-4 my-1 text-h5">{{ this.confirmed }}</p>
          <!-- <p class="red--text my-1 darken-4">{{ '+' + this.global.NewConfirmed }}</p> -->
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="grey--text my-1">Deaths</p>
          <p class="grey--text my-1 text-h5">{{ this.deaths }}</p>
          <!-- <p class="grey--text my-1">{{ '+' + this.global.NewDeaths }}</p> -->
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="green--text my-1 darken-1">Recovered</p>
          <p class="green--text darken-1 my-1 text-h5">{{ this.recovered }}</p>
          <!-- <p class="green--text my-1 darken-1">{{ '+' + this.global.NewRecovered }}</p> -->
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="12" md="6">
        <v-card class="pa-2" outlined tile>
          <confirmed-chart :items="items" :dates="dates"></confirmed-chart>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="6">
        <v-card class="pa-2" outlined tile>
          <client-only>
            <apexchart type="donut" :options="options" :series="dseries"></apexchart>
          </client-only>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
  import _ from 'underscore';
  import apexchart from 'vue-apexcharts';

  export default {
    data() {
      return {
        dseries: [],
        options: {
          labels: ['Confirmed', 'Deaths', 'Recovered']
        },
        confirmed: 0,
        deaths: 0,
        recovered: 0,
        country: '',
        data: []
      }
    },
    components: {
      apexchart
    },
    computed: {
      items() {
        return _.pluck(this.data, 'Confirmed');
      },
      dates() {
        let dates = [];
        _.each(this.data, (item) => {
          dates.push(new Date(item.Date).toDateString());
        });
        return dates;
      }
    },
    async mounted() {
      try {
        const res = await this.$axios.get('api/live/country/' + this.$route.params.slug);
        const item = _.last(res.data);

        this.data = res.data;

        this.country = item.Country;

        this.confirmed = item.Confirmed;
        this.deaths = item.Deaths;
        this.recovered = item.Recovered;

        this.dseries.push(this.confirmed);
        this.dseries.push(this.deaths);
        this.dseries.push(this.recovered);
      } catch (error) {
        console.log(error);
      }
    },
  }

</script>
