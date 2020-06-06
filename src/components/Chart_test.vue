<template>
  <div>
    <h1>Chart</h1>

    <div id="chart">

      <div v-for="(d, index) in benchmarks_data_chart" 
        :key="`d-${index}`"
      >   
        {{ addBubble(d) }}
        <!-- {{ d }}
        <svg>
          <circle 
            :cx="xScale(d.avg)" 
            :cy="yScale(d.parameters)" 
            r="10" 
            stroke="black" 
            stroke-width="3" 
            fill="red"
          />
        </svg> -->

      </div>
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: 'Chart',
  props: {
    benchmarks_data: Array,
    benchmarks_data_chart: Array,
    colorScale: null,
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
      opacityCircles: 0.6,
      },
      svgContainer: Object,
      chartWrapper: Object,
      key_x: "avg",
      key_y: "parameters",
      key_r: "depth_multiplier",
    }
  },
  mounted: function() {
    
    this.svgContainer = d3.select("#chart")
      .append("svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))

    const xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.key_dom(this.key_x))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + (this.settings.height + this.settings.margin.top) + ")")
      .call(d3.axisBottom(xScale))

    const yScale = d3.scaleLinear()
      .range([this.settings.height, 0])
      .domain(this.key_dom(this.key_y))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")")
      .call(d3.axisLeft(yScale));

    this.chartWrapper = this.svgContainer
      .append("g")
      .attr("class", "chordWrapper")
      .attr("transform", "translate(" + this.settings.margin.left + "," + this.settings.margin.top + ")");

    this.chartWrapper
      .append("g")
        .attr("class", "grid")
        .attr("transform", "translate(0," + this.settings.height + ")")
        .call(d3.axisBottom(xScale)
            .ticks(5)
            .tickSize(-this.settings.height)
            .tickFormat("")
        )
    this.chartWrapper.append("g")
        .attr("class", "grid")
        .call(d3.axisLeft(yScale)
            .ticks(5)
            .tickSize(-this.settings.width)
            .tickFormat("")
        )
    this.chartWrapper.append("text")
          .attr("x", (this.settings.width / 2))
          .attr("y", this.settings.height + this.settings.margin.top)
          .attr("dy", "1em")
          .style("text-anchor", "middle")
          .text("Average Time");
    // this.chartWrapper.append("text")
    //       .attr("transform", "rotate(-90)")
    //       .attr("y", 0 - this.settings.margin.left)
    //       .attr("x", 0 - (this.settings.height / 2))
    //       .attr("dy", "1em")
    //       .style("text-anchor", "middle")
    //       .text("# Parameters");

    // const rScale = d3.scaleLinear()
		// 	.range([5,12])
    //   .domain(this.key_dom(this.key_r))

    // var that = this
    // this.chartWrapper.append('g')
    //   .selectAll("dot")
    //   .data(this.benchmarks_data_chart)
    //   .enter()
    //   .append("circle")
    //     .attr("class", function() { return "benchmarks" ; })//+ d[]; })
    //     .style("opacity", this.settings.opacityCircles)
    //     .style("fill", function(d) {return that.colorScale(d.input_size);})
    //     .style("stroke", "white")
    //     .attr("cx", function(d) {return xScale(d.avg);})
    //     .attr("cy", function(d) {return yScale(d.parameters);})
    //     .attr("r", function(d) {return rScale(d.depth_multiplier)});
  },
  methods: {
    addBubble: function(){
      console.log('adding a bubble...', this.chartWrapper)
      this.chartWrapper.append("text")
        .attr("transform", "rotate(-90)")
        .attr("y", 0 - this.settings.margin.left)
        .attr("x", 0 - (this.settings.height / 2))
        .attr("dy", "1em")
        .style("text-anchor", "middle")
        .text("# Parameters");
  },
    xScale: function(){
      d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.key_dom(this.key_x))
    },
    yScale: function() {
      d3.scaleLinear()
      .range([this.settings.height, 0])
      .domain(this.key_dom(this.key_y))
    },
    rScale: function(){ 
      d3.scaleLinear()
			.range([5,12])
      .domain(this.key_dom(this.key_r))
    },
    // chartContainer() {
    //   return {
    //     '--width': this.settings.width,
    //     '--height': this.settings.height
    //   }
    // },
    // addBubble() {
    //   return {
    //     'cx'"10" 
    //       :cy="10"
    //       r="40" 
    //       stroke="black" 
    //       stroke-width="3" 
    //       fill="red"
    //     '--width': this.settings.width,
    //     '--height': this.settings.height
    //   }
    // },
    key_dom: function(key){
      var dom = d3.extent(this.benchmarks_data, function(d) { return parseFloat(d[key]) ; })
      return dom
      
    },
  }
}
</script>

<style>
.chartContainer {
  width: var(--fill);
  height: var(--height);
}
</style>