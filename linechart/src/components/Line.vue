<template id="d3__line">
  <path class="line" ref="line" :style="style"></path>
</template>

<script>
import d3 from 'd3'
export default {
  name : 'Line',
  props:['layout', 'series-data', 'scale' ],
  mounted: function(){
      this.drawLine();
  },
  methods: {
      drawLine: function(){
          var scale = this.scale;

          var line = d3.line()
              .x(function(d) {
                  return scale.x(d.timestamp);
              })
              .y(function(d) {
                  return scale.y(d.value);
              });

        var $line = d3.select(this.$refs.line);

        $line
        .data([this.seriesData.values.filter(function(d){
            return typeof d.value !== typeof null;
        })])
        .attr('d', line);
          
      },
      computed: {
          style: function(){
              return {
                  fill: 'none',
                  stroke: this.scale.color(this.seriesData.id),
                  strokeWidth : 2
              }
          }
      },
      watch :{
          scale: {
              deep: true,
              handler : function(val , oldVal){
                  this.drawLine()
              }
          }
      }
  }
}
</script>
