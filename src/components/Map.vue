<template>
  <div id="container">
    <div class="map" id="map">
      <v-map :zoom="zoom" :center="center">
        <v-tilelayer :url="url"></v-tilelayer>
        <v-marker :lat-lng="center"></v-marker>
      </v-map>
      <!-- <input id="search" type="text" /> -->
    </div>

  </div>
</template>

<script>
export default {
  name: 'Map',
  data() {
    return {
      zoom: 6,
      center: [63.027087, 21.095948],
      url: 'http://{s}.tile.osm.org/{z}/{x}/{y}.png',
    };
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
