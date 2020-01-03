<template v-if="benchmark_data.length >0">
  <div id="app">
    <Chart 
      :benchmark_data="benchmark_data"
    />
  </div>
</template>

<script>
import * as d3 from "d3";

import Chart from '@/components/Chart.vue'
//import Legend from '@/components/Legend.vue'

export default {
  name: 'app',
  components: {
    Chart,
    //Legend,
  },
  data: function(){
    return {
      data_loaded: false,
      benchmark_data: [],
    }
  },
  created: function() {
    var that = this 

    d3.csv("benchmark.csv",
      function(data) {  
        that.benchmark_data.push(data)
      }
    );

    console.log("creatingting parent: " , this.benchmark_data.length)
    this.data_loaded = true
    console.log(this.data_loaded)
  },
  computed: {
    // input_size: function(){
    //   let input_size = this.benchmark_data.map(a => a.input_size).filter((x, i, a) => a.indexOf(x) == i).sort(function(a, b){return parseFloat(a)-parseFloat(b)});
    //   return input_size
    // },
    // colors: function(){
    //   let temp = this.benchmark_data.map(a => parseFloat(a.input_size)).filter((x, i, a) => a.indexOf(x) == i).sort(function(a, b){return a-b});
    //   let color_range = temp.map(x => d3.interpolateOranges((x-temp[0])/(temp[temp-1]-temp[0])));
    //   let input_size = this.benchmark_data.map(a => a.input_size).filter((x, i, a) => a.indexOf(x) == i).sort(function(a, b){return parseFloat(a)-parseFloat(b)});

    //   //Set the color for each region
    //   let colors = d3.scaleOrdinal()
    //           .domain(input_size)
    //           .range(color_range);
    //   return colors
    // },
    
    // param_dom: function(){
    // let param_dom = d3.extent(this.benchmark_data, function(d) { return parseFloat(d.parameters) ; })
    // return param_dom
    // }
  },
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
