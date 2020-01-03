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
    benchmark_data: Array,
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
    console.log("mounting child...")

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

    this.chartWrapper = this.svgContainer
      .append("g")
      .attr("class", "chordWrapper")
      .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")");
    },
  computed: {
    avg_dom: function(){
      console.log(this.benchmark_data.length)
      var dom = d3.extent(this.benchmark_data, function(d) { return parseFloat(d.avg) ; })
      console.log("computing avg dom", dom)
      return dom
    },
    xScale: function() {
      let xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      //.domain([this.avg_dom[0], this.avg_dom[1]])
      .domain(this.avg_dom)
      return xScale
    },
    yScale: function() {
      let yScale = d3.scaleLinear()
      .range([0, this.settings.height])
      //.domain(d3.extent(this.benchmark_data, function(d) { return parseFloat(d.avg) ; }));
      .domain([0, 4000])
      return yScale
    },
    rScale: function (){
      let rScale = d3.scaleSqrt()
        .domain(d3.extent(this.benchmark_data, function(d) { return 5*d.depth_multiplier;}));
      return rScale
    }
  }
}
</script>