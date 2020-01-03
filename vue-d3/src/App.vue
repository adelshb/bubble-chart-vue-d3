<template>
  <div id="app">
    <Chart 
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

    d3.csv("spotify_top50_2019.csv",
      function(data) {  
        that.spotify_top50_2019.push(data)
      }
    );

    console.log("parent created, data length: ", this.spotify_top50_2019.length)
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
