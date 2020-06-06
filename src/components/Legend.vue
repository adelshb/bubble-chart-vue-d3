<template>
  <div>
    <h1>Legend</h1>
    Click on the legend elements
    <ul class="removeBulletAddTick">
      <li 
        v-for="(input, index) in input_size" 
        :key="`input-${index}`"
      >

        <div class="legendElementWrapper"
          @click="sendFilterInput(input, index)"
        >
          <svg width="20" height="20">
            <rect :style="cssRectFill(input)" width="20" height="30"/>
          </svg>
          {{ input }}
        </div>
        
      </li>
    </ul>
  </div>
</template>

<script>
//import * as d3 from "d3";

export default {
  name: 'leg',
  props: {
    benchmarks_data: Array,
    colorScale: null,
    },
  data: function() {
    return {
      input_size: [],
      clickInput: [],
    }
  },
  created: function() {
    this.input_size = this.benchmarks_data.map(a => a.input_size).filter((x, i, a) => a.indexOf(x) == i).sort(function(a, b){return parseFloat(a)-parseFloat(b)});
    this.clickInput = new Array(this.input_size.length).fill(false)
  },
  mounted: function() {
  },
  methods: {
    sendFilterInput(input, index) {
      if ( this.clickInput[index] == false ) {
        this.clickInput[index] = true
        this.$emit('inputChange', input)
      } else if ( this.clickInput[index] == true ) {
        this.clickInput[index] = false
        this.$emit('inputChangeBack', input)
      }
    },
    cssRectFill(legItem) {
      return {
        '--fill': this.colorScale(legItem)
      }
    }
  },
  computed: {
  }
}
</script>

<style>
ul {
  text-align: left;
}
.legendElementWrapper {
  cursor: pointer;
}
rect {
  fill: var(--fill);
}
</style>