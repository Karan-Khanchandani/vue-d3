<template id="d3__series">
  <g class="d3__series">
    <d3__area
              :layout="layout"
              :series-data="this.seriesData"
              :scale="this.scale">
    </d3__area>
    <d3__line
              :layout="layout"
              :series-data="this.seriesData"
              :scale="this.scale">
    </d3__line>
    <d3__scatter
                 :layout="layout"
                 :series-data="this.seriesData"
                 :scale="this.scale">
    </d3__scatter>
  </g>
</template>

<script>

import Line from './Line'
import Area from './Area'
import Scatter from './Scatter'

export default {
  name: 'Series',
  components: {
    'd3__area': Area,
    'd3__line': Line,
    'd3__scatter': Scatter
  },
  props: ['layout', 'series-data', 'scale']
}
</script>
