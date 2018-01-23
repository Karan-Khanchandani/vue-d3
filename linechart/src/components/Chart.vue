
<template id="d3__chart">
  <svg :view-box.camel="viewBox" preserveAspectRatio="xMidYMid meet">
    <g class="d3__stage" :style="stageStyle">
      <d3__axis
                v-for="axis in unique(axes)"
                :key="axis.id"
                :axis="axis"
                :layout="layout"
                :scale="scale"
                ></d3__axis>
      <d3__series
                  v-for="seriesData in chartData"
                  :key="seriesData"
                  :series-data="seriesData"
                  :layout="layout"
                  :scale="scale"></d3__series>
    </g>
  </svg>
</template>

<script>
import Axis from './Axis'
import Series from './Series'
import * as d3 from 'd3'
import * as _ from 'underscore'

export default {
  name: 'Chart',
  components: {
    'd3__axis': Axis,
    'd3__series': Series
  },
  props: [
    'axes', // Chart axes
    'layout', // Dimensions for the chart and margins
    'chartData' // Data for plotting
  ],
  computed: {

    // SVG viewbox
    viewBox: function () {
      var outerWidth = this.layout.width + this.layout.marginLeft + this.layout.marginRight
      var outerHeight = this.layout.height + this.layout.marginTop + this.layout.marginBottom
      return '0 0 ' + outerWidth + ' ' + outerHeight
    },

    // Stage
    stageStyle: function () {
      return {
        'transform': 'translate(' + this.layout.marginLeft + 'px,' + this.layout.marginTop + 'px)'
      }
    }
  },
  data: function () {
    return {
      scale: {}
    }
  },
  mounted: function() {
    //console.log(this.layout);
    debugger
    this.scale = {
        x: this.getScaleX(),
        y: this.getScaleY(),
        color: d3.scaleOrdinal()
          .range(['#159078', '#999999'])
          .domain(['Current', 'Previous'])
      }
  },
  methods: {

    unique: function (data) {
      return _.uniq(data)
    },

    // Get x-axis scale
    getScaleX: function () {
      return d3.scaleTime()
        .range([0, this.layout.width])
        .domain(d3.extent(this.chartData, function (d) {
          debugger
          return d3.utcParse('%Y-%m-%dT%H:%M:%S')(d[0]).setHours(0, 0, 0, 0)
        }))
    },

    // Get y-axis scale
    getScaleY: function () {
      return d3.scaleLinear()
        .range([this.layout.height, 0])
        .domain([
          0,
          d3.max(this.chartData, function (d) {
            return d3.max(d.values, function (e) {
              return e.value
            })
          })
        ])
    }
  },
  watch: {
    // Watch for layout changes
    layout: {
      deep: true,
      handler: function (val, oldVal) {
        this.scale.x = this.getScaleX()
        this.scale.y = this.getScaleY()
      }
    }
  }
}
</script>
