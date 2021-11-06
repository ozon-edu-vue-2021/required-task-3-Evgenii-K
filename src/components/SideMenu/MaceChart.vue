<template>
  <Doughnut
    ref="chart"
  />
</template>

<script>
import legend from "@/assets/data/legend.json";
import { Doughnut } from 'vue-chartjs'

export default {
  name: 'MakeChart',
  components: {
    Doughnut
  },
  data() {
    return {
      legend: [],
    };
  },
  created() {
    this.loadLegend();
  },
  mounted() {
    this.makeChart()
  },
  methods: {
    loadLegend() {
      this.legend = legend;
    },
    makeChart() {
      const chartData = {
        labels: this.legend.map(legendItem => legendItem.text),
        datasets: [
          {
            label: 'Легенда',
            backgroundColor: this.legend.map(legendItem => legendItem.color),
            data: this.legend.map(legendItem => legendItem.counter)
          }
        ]
      }
      const options = {}
      this.$refs.chart.renderChart(chartData, options)
    },
  }
}
</script>

<style>

</style>