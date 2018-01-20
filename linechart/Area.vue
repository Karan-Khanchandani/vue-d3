<template id = "d3__area">
  <path class="area" ref="area" :style="style"></path>
</template>

<script>
export default {
  name: 'Area',
  props: ['layout', 'series-data', 'scale'],
  mounted: function(){
      this.drawArea();
  },
  methods: {
      drawArea: function(){
          var scale = this.scale;
          var area = d3.area()
          .x(function(d) {
              return scale.x(d.timestamp);
          })
          .y0(scale.y(0))
          .y1(function(d) {
              return scale.y(d.value);
          })

          var $area = d3.select(this.$refs.area);

          $area.
          datum(this.seriesData.values.filter( function(d) {
              return typeof d.value !== typeof null;
          }))
          .attr('d', area);
      }
  },
  computed: {
      style: function(){
          return {
              fill: this.scale.color(this.seriesData.id),
        fillOpacity: 0.25
          }
      }
  },
  watch: {
      scale: {
      deep: true,
      handler: function(val, oldVal) {
        this.drawArea();
      }
    }
  }
}
</script>
