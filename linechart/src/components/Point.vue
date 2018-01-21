<template id="d3__point">
  <circle class="point" ref="point" :style="style"></circle>
</template>

<script>
import d3 from 'd3'

export default {
  name: 'd3_point',
  props: ['layout', 'point-data', 'scale', 'series-id'],
  data () {

  },
  mounted () {
      this.drawPoint();
  },
  methods : {
      drawPoint: function() {
          var scale = this.scale;

          var $point = d3.select(this.$refs.point);
          $point
          .datum(this.pointData)
          .attr('cx', function(d){
              return scale.x(d.timestamp);
          })
          .attr('cy', function(d){
              return scale.y(d.value);
          })
          .attr('r', 5);
      }
  },
  computed: {
      style: function() {
        return{
            fill: '#fff',
            stroke: this.scale.color(this.seriesId),
            strokeWidth: 2
        }
      }
  },
  watch:{
      scale: {
          deep: true,
          handler: function(val, oldVal){
              this.drawPoint()
          }
      }
  }
}
</script>
