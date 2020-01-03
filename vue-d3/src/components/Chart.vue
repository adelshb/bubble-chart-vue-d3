<template>
  <div>
    <h1>Chart</h1>
      <div id="chart">
      </div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'Chart',
  props: {
    spotify_top50_2019: Array,
    },
  data: function() {
    return {
      settings: {
        margin: {
          top: 20, 
          right: 10, 
          bottom: 40, 
          left: 70
        },
      width: 600,
      height: 400,
      opacityCircles: 0.5,
      },
      svgContainer: Object,
      chartWrapper: Object,
    }
  },  
  mounted: function() {

    this.svgContainer = d3.select("#chart")
      .append("svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + (this.settings.height + this.settings.margin.top) + ")")
      .call(d3.axisBottom(this.xScale))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")")
      .call(d3.axisLeft(this.yScale));

    // this.chartWrapper = this.svgContainer
    //   .append("g")
    //   .attr("class", "chordWrapper")
    //   .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")");
    },
  computed: {
    popularity_dom: function(){
      console.log("computing popularity domain, data length...",this.spotify_top50_2019.length)
      var dom = d3.extent(this.spotify_top50_2019, function(d) { return parseFloat(d.Popularity) ; })
      return dom
    },
    xScale: function() {
      let xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.popularity_dom)
      return xScale
    },
    yScale: function() {
      let yScale = d3.scaleLinear()
      .range([0, this.settings.height])
      //.domain(d3.extent(this.spotify_top50_2019, function(d) { return parseFloat(d.) ; }));
      .domain([0, 4000])
      return yScale
    },
    // rScale: function (){
    //   let rScale = d3.scaleSqrt()
    //     .domain(d3.extent(this.spotify_top50_2019, function(d) { return 2*d.Length;}));
    //   return rScale
    // }
  }
}
</script>