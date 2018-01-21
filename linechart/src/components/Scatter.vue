<template id = "d3__scatter">
  <g class = "points">
      <d3__point
        v-for="pointData in seriesData.values"
        :key="pointData.id"
        v-if="typeof pointData.value !== typeof null"
        :series-id="seriesData.id"
        :point-data="pointData"
        :layout="layout"
        :scale="scale"></d3__point>
    </g>
</template>
<script>
import Point from './Point';
export default {
  name: 'scatter',
  components:{
      'd3__point': Point
  },
  data (){
      return {}
  },
  props: ['layout', 'series-data', 'scale'],
  watch: {
      scale: {
          deep:true,
          handler: function(){}
      }
  }
}
</script>
