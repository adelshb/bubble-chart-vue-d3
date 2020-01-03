<template>
  <div id="app">
    <Chart
      v-if="spotify_top50_2019.length > 0"
      :spotify_top50_2019="spotify_top50_2019"
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
      spotify_top50_2019: []
    }
  },
  created: function() {
    var that = this

    d3.csv("spotify_top50_2019.csv", function(loadedRows) {
      that.spotify_top50_2019.push(loadedRows)
    });
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
