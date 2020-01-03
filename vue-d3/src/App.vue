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

    console.log("creatingt parent: " , this.benchmark_data.length)
    this.data_loaded = true
    console.log(this.data_loaded)
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
