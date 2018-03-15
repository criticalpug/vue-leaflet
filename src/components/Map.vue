

<template>
  <div class="container">
    <div class="map" id="map">
      <v-map :zoom=13 :center="[61.49911, 23.78712]">
        <v-tilelayer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></v-tilelayer>
        <v-marker :lat-lng="[61.49911, 23.78712]"></v-marker>
      </v-map>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Map',
  data() {
    return {
      msg: 'Here be maps',
    };
  },
};
</script>

<style>

  @import "../../node_modules/leaflet/dist/leaflet.css";
</style>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .container {
    display: flex;
    justify-content: center;
    align-items:center;
  }
  #map {
    height: 50vh;
    width: 50vw;
  }
</style>
