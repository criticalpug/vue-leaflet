<template>
  <div id="container">
    <div class="map" id="map" v-on:click="closeMenu">
      <v-map :zoom="zoom" :center="center">
        <v-tilelayer :url="url"></v-tilelayer>
        <v-ais v-for="(ship) in ships"
          :key="ship.id"
          :lat-lng="ship.position"
          :visible="ship.visible"
          :options="ship.options">
          <v-popup :content="ship.tooltip"></v-popup>
        </v-ais>
        <v-poly v-for="(ship) in ships"
          :visible="ship.visible"
          :key="`route-`+ ship.id"
          :lat-lngs="ship.route">
        </v-poly>
      </v-map>
    </div>
    <div class="menu" v-bind:class="{ 'menu-open': menuOpen }">
      <div class="vessels">
        <div :key="`check-`+ ship.id" v-for="(ship) in ships">
        <label><input type="checkbox" v-model="ship.visible">{{ship.id}}</label>
        </div>
      </div>
    </div>
    <button v-on:click="toggleMenu" id="burger">=</button>
  </div>
</template>

<script>
import Vue2Leaflet from 'vue2-leaflet';
import Vue2LeafletTracksymbol from 'vue2-leaflet-tracksymbol';
import * as R from 'ramda';

import MarkerPopup from './MarkerPopup';

const AIS = require('../data/ais.json');

// const Kiisla = 230956000;

const degreesToRadians = degrees => Math.round(degrees * (Math.PI / 180));
const knotsInMS = knots => knots * 0.514444444;

const getOptions = ais => ({
  trackId: ais.mmsi,
  fill: true,
  fillColor: '#00ffff',
  fillOpacity: 1.0,
  stroke: true,
  color: '#000000',
  opacity: 1.0,
  weight: 1.0,
  speed: knotsInMS(ais.sog), // meter per second
  course: degreesToRadians(ais.cog), // radians
  heading: degreesToRadians(ais.heading), // radians
  size: 24,
  defaultSymbol: [0.75, 0, 0.5, 0.3, -0.5, 0.3, -0.25, 0, -0.5, -0.3, 0.5, -0.3],
  data: { name: 'Boat 2', custom: 'other info' },
});

/* eslint-disable no-param-reassign */
const accumulator = (obj, ais) => {
  if (!obj.id) {
    obj.id = ais.mmsi;
    obj.route = [];
    obj.tooltip = `MMSI: <a href="https://www.marinetraffic.com/en/ais/details/ships/mmsi:${ais.mmsi}" target="_blank">${ais.mmsi}</a>`;
    obj.draggable = false;
    obj.visible = true;
    obj.options = getOptions(ais);
  }

  obj.position = { lat: ais.lat, lng: ais.lon };
  obj.route.push({ lng: ais.lon, ...ais });

  return obj;
};

const groupedAIS = R.pipe(
  R.groupBy(R.prop('mmsi')),
  R.map(list => R.reduce(accumulator, {}, list)),
)(AIS);

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
      center: [63.027087, 21.095948],
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
      ships: [],
      route: [],
      menuOpen: false,
    };
  },
  methods: {
    toggleMenu() {
      this.menuOpen = !this.menuOpen;
    },
    closeMenu() {
      this.menuOpen = false;
    },
  },
  mounted() {
    /* eslint-disable no-console */
    console.log('Mounted');

    // this.ships = [AISKiisla.map(ais => convertAisToShip(ais))[AISKiisla.length - 1]];
    this.ships = groupedAIS;
  },
  created() {
    /* eslint-disable no-console */
    console.log('Created');
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

.menu {
  position: absolute;
  background-color: white;
  top: 0;
  left: 0;
  width: 0;
  height: 100%;
  z-index: 9999;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}

.menu-open {
  width: 200px;
}

#burger {
  background-color: white;
  width: 30px;
  height: 30px;
  position: absolute;
  top: 10px;
  right: 10px;
  z-index: 9999;
  cursor: pointer;
  outline: none;
  border:none;
  box-shadow:none;
}
</style>
