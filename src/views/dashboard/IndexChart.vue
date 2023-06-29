<template>
  <baidu-map class="map" center="北京" :scroll-wheel-zoom="true" :min-zoom="5" :max-zoom="18" :continuous-zoom="true">
    <bml-marker-clusterer :averageCenter="true">
      <bm-marker v-for="marker of markers" :position="{ lng: marker.lng, lat: marker.lat }"
        @click="infoWindowOpen(marker)">
        <bm-info-window :show="marker.show" @close="infoWindowClose(marker)" @open="infoWindowOpen(marker)">{{ marker.name
        }}</bm-info-window>
      </bm-marker>
    </bml-marker-clusterer>
  </baidu-map>
</template>

<script>
import { BmlMarkerClusterer } from 'vue-baidu-map'
import { getAction } from '@/api/manage'

export default {
  name: "IndexChart",
  components: {
    BmlMarkerClusterer,
  },
  data() {
    return {
      markers: [],
    }
  },
  created() {
    console.log("首页！！！");
    getAction(`/restaurant/all`, {}).then(res => {
      if (res.success) {
        for (let i = 0; i < res.result.length; i++) {
          const restautrant = res.result[i];
          const position = { lng: restautrant.lng, lat: restautrant.lat, name: restautrant.name, id: restautrant.id, show: false }
          this.markers.push(position);
        }
      } else {

      }
    })

  },
  mounted() {
  },
  methods: {
    infoWindowClose(marker) {
      marker.show = false;
    },
    infoWindowOpen(marker) {
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