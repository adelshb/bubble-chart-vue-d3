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
      opacityCircles: 0.6,
      },
      svgContainer: Object,
      chartWrapper: Object
    }
  },  
  mounted: function() {

    this.svgContainer = d3.select("#chart")
      .append("svg")
      .attr("width", (this.settings.width + this.settings.margin.left + this.settings.margin.right))
      .attr("height", (this.settings.height + this.settings.margin.top + this.settings.margin.bottom))

    const xScale = d3.scaleLinear()
      .range([0, this.settings.width])
      .domain(this.key_dom("Popularity"))
    this.svgContainer.append("g")
      .attr("transform", "translate(" + this.settings.margin.left + "," + (this.settings.height + this.settings.margin.top) + ")")
      .call(d3.axisBottom(xScale))

    const yScale = d3.scaleLinear()
      .range([this.settings.height, 0])
      .domain(this.key_dom("Length."))
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
          .text("Popularity");
    this.chartWrapper.append("text")
          .attr("transform", "rotate(-90)")
          .attr("y", 0 - this.settings.margin.left)
          .attr("x", 0 - (this.settings.height / 2))
          .attr("dy", "1em")
          .style("text-anchor", "middle")
          .text("Length");

    const rScale = d3.scaleLinear()
			.range([5,15])
      .domain(this.key_dom("Beats.Per.Minute"))

    var colorScale = d3.scaleOrdinal(d3.schemeCategory10)

    this.chartWrapper.append('g')
      .selectAll("dot")
      .data(this.spotify_top50_2019)
      .enter()
      .append("circle")
        .attr("class", function(d) { return "Spotify Top50 2019" + d["Track.Name"]; })
        .style("opacity", this.settings.opacityCircles)
        .style("fill", function(d) {return colorScale(d.Genre);})
        .style("stroke", "white")
        .attr("cx", function(d) {return xScale(d.Popularity);})
        .attr("cy", function(d) {return yScale(d["Length."]);})
        .attr("r", function(d) {return rScale(d["Beats.Per.Minute"])});
    },

  computed: {
    rScale: function (key){
      let rScale = d3.scaleSqrt()
        .domain(d3.extent(this.spotify_top50_2019, function(d) { return d[key];}));
      return rScale
    }
  },
  methods: {
    key_dom: function(key){
      return d3.extent(this.spotify_top50_2019, function(d) { return parseFloat(d[key]) ; })
    },
  }
}
</script>