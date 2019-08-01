<template>
    <div id='map-container'>
    </div>
</template>

<script>
// import wifiData from '../assets/sapporo-wifi.json'

import 'leaflet/dist/leaflet.css'
import L from 'leaflet'
// import { setTimeout } from 'timers';
// import { Script } from 'vm'
delete L.Icon.Default.prototype._getIconUrl

L.Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png')
})

export default {
  props: [
    'lat',
    'lon'
  ],
  computed: {
    latitude: function () {
      return this.lat
    },
    longitude: function () {
      return this.lon
    }
  },
  data: function () {
    return {
      map: null,
      currentCircle: null,
      zoom: 16
    }
  },
  mounted () {
    // 地図作成 札幌駅
    this.map = L.map('map-container')
      .setView([this.latitude, this.longitude], 16)
      .addLayer(
        L.tileLayer('https://{s}.tile.osm.org/{z}/{x}/{y}.png', {
          attribution:
            'Map data &copy; <a href="https://openstreetmap.org">OpenStreetMap</a>'
        })
      )

    // 現在位置へ
    // if (navigator.geolocation) {
    //   navigator.geolocation.getCurrentPosition(position => {
    // setTimeout(() => {
    //   map.panTo([this.latitude, this.longitude])
    // }, 1000)

    this.currentCircle = L.circle(
      [this.latitude, this.longitude],
      {
        color: '#0099AA',
        fillColor: '#09A',
        fillOpacity: 0.25,
        radius: 100
      }
    ).addTo(this.map)
    // })
    navigator.geolocation.watchPosition(this.updateCurrentPosition)
    // } else {
    //   console.log('Not Geo')
    // }
  },
  methods: {
    updateCurrentMap: function () {
      if (this.map) {
        this.map.panTo([this.latitude, this.longitude])
      }
    },
    updateCurrentPosition: function (position) {
      this.currentCircle.setLatLng([this.latitude, this.longitude])
    }
  }
}
</script>

<style scoped>
#map-container {
    height: auto;
}

.leaflet-container {
  z-index: 0;
}
</style>
