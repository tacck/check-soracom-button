<template>
  <div id="main">
    <PositionComponent></PositionComponent>
    <Distance></Distance>
    <Map v-bind:lat="lat"  v-bind:lon="lon"></Map>
  </div>
</template>

<script>
import Map from './Map'
import PositionComponent from './PositionComponent'
import Distance from './Distance'

export default {
  components: {
    Map,
    PositionComponent,
    Distance
  },
  data: function () {
    return {
      lat: 43.0680165,
      lon: 141.3519007
    }
  },
  mounted: function () {
    console.log(this.lat, this.lon)
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        console.log(this.lat, this.lon)
      })
    }
  }
}
</script>

<style scoped>
#main {
  height: 100%;
  display: grid;
  grid-template-rows: 8em 4em 1fr;
}
</style>
