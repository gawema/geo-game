<template>
  <h1>StreetView.vue</h1>
  <!-- <div id="street-view">
    {{ this.randCoord }}
  </div> -->
  <div id="map"></div>
  <div id="pano"></div>
</template>

<script>
// import google from "googleapis";
import locations from "./../assets/locations";

export default {
  name: "StreetView",
  data() {
    return {
      //   googleApiKey: process.env.GOOGLE_API_KEY,
      randCoord: locations[Math.floor(Math.random() * locations.length)],
    };
  },
  mounted() {
    window.checkAndAttachMapScript(this.createMap);
  },
  methods: {
    createMap() {
      const fenway = { lat: 42.345573, lng: -71.098326 };
      const map = new window.google.maps.Map(document.getElementById("map"), {
        center: fenway,
        zoom: 14,
      });
      const panorama = new window.google.maps.StreetViewPanorama(
        document.getElementById("pano"),
        {
          position: fenway,
          pov: {
            heading: 34,
            pitch: 10,
          },
        }
      );
      map.setStreetView(panorama);
    },
  },
};
</script>
<style lang="css">
/* #map {
  height: 400px;
  width: 400px;
} */
#pano {
  height: 400px;
  width: 400px;
}
</style>
