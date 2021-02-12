<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="6" md="6">
        <p class="my-1 border-l-4 p-2 text-gray-700 border-red-500">Today Overview</p>
      </v-col>
      <v-col cols="12" sm="6" md="6">
        <p class="my-1 text-right text-gray-500" v-text="(new Date()).toDateString()"></p>
      </v-col>
    </v-row>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="red--text my-1 darken-4">Confirmed</p>
          <p class="red--text darken-4 my-1 text-h5">{{ this.global.TotalConfirmed }}</p>
          <p class="red--text my-1 darken-4">{{ '+' + this.global.NewConfirmed }}</p>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="grey--text my-1">Deaths</p>
          <p class="grey--text my-1 text-h5">{{ this.global.TotalDeaths }}</p>
          <p class="grey--text my-1">{{ '+' + this.global.NewDeaths }}</p>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="4">
        <v-card class="pa-2 text-center" outlined tile>
          <p class="green--text my-1 darken-1">Recovered</p>
          <p class="green--text darken-1 my-1 text-h5">{{ this.global.TotalRecovered }}</p>
          <p class="green--text my-1 darken-1">{{ '+' + this.global.NewRecovered }}</p>
        </v-card>
      </v-col>
    </v-row>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="12" md="12">
        <v-card class="pa-2" outlined tile>
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
              <tr>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                  Country Code
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                  Country
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                  Confirmed
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                  Deaths
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-700 uppercase tracking-wider">
                  Recovered
                </th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
              <tr v-for="(item,i) in items" :key="i">
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                  {{ item.CountryCode }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm">
                  <nuxt-link :to="{name: 'country-slug',params: {slug: item.Slug}}" class="text-black underline">{{ item.Country }}</nuxt-link>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-red-700">{{ item.TotalConfirmed }}</div>
                  <div class="text-sm text-red-400">{{ '+' + item.NewConfirmed }}</div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-blue-700">{{ item.TotalDeaths }}</div>
                  <div class="text-sm text-blue-400">{{ '+' + item.NewDeaths }}</div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-green-700">{{ item.TotalRecovered }}</div>
                  <div class="text-sm text-green-400">{{ '+' + item.NewRecovered }}</div>
                </td>
              </tr>
            </tbody>
          </table>
        </v-card>
      </v-col>
    </v-row>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        items: [],
        global: {
          TotalConfirmed: 0,
          NewConfirmed: 0,
          TotalDeaths: 0,
          NewDeaths: 0,
          TotalRecovered: 0,
          NewRecovered: 0,
        }
      }
    },
    async mounted() {
      try {
        const res = await this.$axios.$get('api/summary');
        this.global = res.Global;
        this.items = res.Countries;
      } catch (error) {
        console.log(error);
      }
    },
  }

</script>

<style>
  .v-application a{
    color: black;
  }
</style>
