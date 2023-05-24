<template>
  <highcharts :options="chartOptions"></highcharts>
</template>

<script>
import HighchartsVue from 'highcharts-vue'
import {Chart} from 'highcharts-vue'
export default {
  name: "Chart",
  components: {
    highcharts: Chart
  },
  data() {
    return {
      chartOptions: {
        series: [{
          data: [1,2,30] // sample data
        }]
      }
    }
  }

}
</script>

<style scoped>

</style>