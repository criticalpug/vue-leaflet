<template>
  <div id="container">
    <div class="map" id="map">
      <v-map :zoom="zoom" :center="center">
        <v-tilelayer :url="url"></v-tilelayer>
        <v-ais v-for="ship in ships"
          :key="ship.id"
          :lat-lng="ship.position"
          :visible="ship.visible"
          :options="ship.options">
          <v-popup :content="ship.tooltip"></v-popup>
          <!-- <v-tooltip :content="ship.tooltip"></v-tooltip> -->
        </v-ais>
      </v-map>
    </div>

  </div>
</template>

<script>
import Vue2Leaflet from 'vue2-leaflet';
import Vue2LeafletTracksymbol from 'vue2-leaflet-tracksymbol';
import MarkerPopup from './MarkerPopup';

export default {
  name: 'Map',
  components: {
    'v-map': Vue2Leaflet.Map,
    'v-marker': Vue2Leaflet.Marker,
    'v-tilelayer': Vue2Leaflet.TileLayer,
    'v-marker-popup': MarkerPopup,
    'v-poly': Vue2Leaflet.Polyline,
    'v-group': Vue2Leaflet.LayerGroup,
    'v-tooltip': Vue2Leaflet.Tooltip,
    'v-popup': Vue2Leaflet.Popup,
    'v-ais': Vue2LeafletTracksymbol,
  },
  data() {
    return {
      zoom: 6,
      marker: [63.027087, 21.095948],
      center: [63.027087, 21.095948],
      title: 'title',
      text: 'text',
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      ships: [
        {
          id: '230662000',
          position: { lat: 63.11851333333333, lng: 20.124348333333334 },
          tooltip: 'MMSI: 230662000',
          draggable: true,
          visible: true,
          options: {
            trackId: 123,
            fill: true,
            fillColor: '#00ffff',
            fillOpacity: 1.0,
            stroke: true,
            color: '#000000',
            opacity: 1.0,
            weight: 1.0,
            speed: 30, // meter per second
            course: 1.0, // radians
            heading: 1.0, // radians
            size: 24,
            defaultSymbol: [0.75, 0, 0.5, 0.3, -0.5, 0.3, -0.25, 0, -0.5, -0.3, 0.5, -0.3],
            data: { name: 'Boat 2', custom: 'other info' },
          },
        },
      ],
    };
  },
  // methods: {
  //   zoomChanged: function (event) {
  //     this.zoom = event.target.getZoom();
  //   },
  // },
  mounted() {

  },
};
</script>

<style>
@import "../../node_modules/leaflet/dist/leaflet.css";
</style>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#container {
  height: 100%;
}

#map {
  height: 100%;
  width: 100%;
}
</style>
