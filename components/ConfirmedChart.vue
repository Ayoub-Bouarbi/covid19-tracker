<template>
  <client-only>
    <apexchart type="area" :options="chartOptions" :series="series"></apexchart>
  </client-only>
</template>

<script>
  import apexchart from 'vue-apexcharts';

  export default {
    props: ['items', 'dates'],
    components: {
      apexchart
    },
    computed: {
      series(){
        return [{
          name: "STOCK ABC",
          data: this.items
        }];
      },
      chartOptions(){
        return {
          title: {
            text: 'Fundamental Analysis of Confirmed Cases',
            align: 'left'
          },
          subtitle: {
            text: 'Cases Movements',
            align: 'left'
          },
          xaxis: {
            categories: this.dates
          },
        }
      }
    },
  }

</script>
