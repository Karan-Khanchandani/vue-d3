<template id="d3__axis">
  <g :class="[classList]" ref="axis" :style="style"></g>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'Axis',
  props: ['axis', 'layout', 'scale'],
  data: function () {
    return {
      classList: ['axis'].concat(this.getAxisClasses())
    }
  },
  mounted: function () {
    this.drawAxis()
  },
  computed: {
    style: function () {
      return {
        transform: this.getAxisTransform()
      }
    }
  },
  methods: {
    getAxisClasses: function () {
      var axis = {
        top: 'x',
        bottom: 'x',
        left: 'y',
        right: 'y'
      }
      return [this.axis, axis[this.axis]]
    },
    drawAxis: function () {
      var $axis = d3.select(this.$refs.axis)
      var scale = this.scale
      var axisGenerator = {
        top: d3.axisTop(scale.x).tickFormat(d3.tickFormat('%b %d')),
        right: d3.axisRight(scale.y),
        bottom: d3.axisBottom(scale.x).tickFormat(d3.timeFormat('%b %d')),
        left: d3.axisLeft(scale.y)
      }

      $axis.call(axisGenerator[this.axis])
    },
    getAxisTransform: function () {
      var axisOffset = {
        top: { x: 0, y: 0 },
        right: { x: this.layout.width, y: 0 },
        bottom: { x: 0, y: this.layout.height },
        left: { x: 0, y: 0 }
      }

      return 'translate(' + axisOffset[this.axis].x + 'px, ' + axisOffset[this.axis].y + 'px)'
    }

  },
  watch: {
    // Changes to scale means we have to redraw the line!
    scale: {
      deep: true,
      handler: function (val, oldVal) {
        this.drawAxis()
      }
    }
  }

}
</script>
