<template>
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
    window.addEventListener('load', () => {
        document.querySelector('[title="Open this area in Google Maps (opens a new window)"]').remove();
    })

    
  },
  methods: {
    createMap() {
        const split = this.randCoord.split(',');
        let lati = Number(split[0]);
        let longi = Number(split[1]);
        const fenway = { lat: lati, lng: longi };
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
            zoom: 1,
            addressControl: false,
            showRoadLabels: false,
            fullscreenControl: false,
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
  height: 100vh;
}
</style>
