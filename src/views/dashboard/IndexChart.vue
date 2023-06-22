<template>
  <baidu-map class="map" center="北京" :scroll-wheel-zoom="true" :min-zoom="5" :max-zoom="18" :continuous-zoom="true">
    <bml-marker-clusterer :averageCenter="true">
      <bm-marker v-for="marker of markers" :position="{ lng: marker.lng, lat: marker.lat }" @click="infoWindowOpen(marker)">
        <bm-info-window :show="marker.show" @close="infoWindowClose(marker)" @open="infoWindowOpen(marker)">{{marker.name}}</bm-info-window>
      </bm-marker>
    </bml-marker-clusterer>
  </baidu-map>
</template>

<script>
import {BmlMarkerClusterer} from 'vue-baidu-map'

export default {
  name: "IndexChart",
  components: {
    BmlMarkerClusterer,
  },
  data() {
    return {
      markers:[],
    }
  },
  created() {
    for (let i = 0; i < 300; i++) {
      const position = {lng: Math.random() * 40 + 85, lat: Math.random() * 30 + 21, name:"久久丫直营店"+i, id: i, show: false}
      this.markers.push(position);
    }
  },
  mounted() {
  },
  methods: {
    infoWindowClose(marker){
      marker.show = false;
    },
    infoWindowOpen(marker){
      marker.show = true;
    },
   
  }
}
</script>

<style lang="less" scoped>
.map {
  width: 100%;
  height: 800px;
}
</style>