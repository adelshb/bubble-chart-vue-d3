<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col >
          <Chart 
            v-if="benchmarks_data_chart.length > 0"
            :benchmarks_data="benchmarks_data"
            :benchmarks_data_chart="benchmarks_data_chart"
            :colorScale="colorScale"
            :key="chartReloadKey"
          />
        </b-col>
        <b-col sm="2">
          <Legend 
            v-if="benchmarks_data.length > 0"
            :benchmarks_data="benchmarks_data"
            :input_size="input_size"
            :colorScale="colorScale"
            @inputChange="filterInput"
            @inputChangeBack="filterInputBack"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import * as d3 from "d3";

import Chart from '@/components/Chart.vue'
import Legend from '@/components/Legend.vue'

export default {
  name: 'app',
  components: {
    Chart,
    Legend,
  },
  data: function(){
    return {
      benchmarks_data: [],
      benchmarks_data_chart: [],
      input_size: [],
      chartReloadKey: 0,
      colorScale: null,
    }
  },
  created: function() {
    var that = this 

    d3.csv("benchmark.csv",
      function(data) {  
        that.benchmarks_data.push(data)
      }
    );
    this.benchmarks_data_chart = this.benchmarks_data
    this.colorScale = d3.scaleOrdinal(d3.schemeCategory10)
    this.benchmarks_data_chart = this.benchmarks_data
  },
  methods: {
    filterInput (input) {
      this.benchmarks_data_chart = this.benchmarks_data_chart.filter(function(d){return d.input_size != input;})
      this.chartReloadKey += 1
    },
    filterInputBack (input) {
      this.benchmarks_data_chart = this.benchmarks_data_chart.concat(this.benchmarks_data.filter(function(d){return d.input_size == input;}))
      this.chartReloadKey += 1
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
