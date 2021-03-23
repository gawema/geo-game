<template>
  <div id="map" class="minimap-container"></div>
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
    const split = this.randCoord.split(',');
    let lati = Number(split[0]);
    let longi = Number(split[1]);
    this.coords = { lat: lati, lng: longi }; 
    window.checkAndAttachMapScript(this.createMap);
    window.addEventListener('load', () => {
        document.querySelector('[title="Open this area in Google Maps (opens a new window)"]').remove();
        var cheatElements = document.getElementsByClassName("gm-style-cc")
        while(cheatElements.length > 0){
          cheatElements[0].parentNode.removeChild(cheatElements[0]);
        }
    })

    
  },
  methods: {
    createMap() {
      // initialize a location in streetview

        const map = new window.google.maps.Map(document.getElementById("map"), {
            center: this.coords,
            zoom: 14,
        });
        const panorama = new window.google.maps.StreetViewPanorama(
            document.getElementById("pano"),
            {
            position: this.coords,
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
      this.checkCoords()
    },
    checkCoordDistance(){
      // check coordinates of user selection on map, and compare to original coords
         console.log(this.coords)
    },
  },
};
</script>
<style scoped>
.minimap-container {
  height: 400px;
  width: 400px;
  position: absolute;
  bottom: 0;
  background-color: RED;
  right: 0;
  z-index: 1;
  margin-right: 75px;
  margin-bottom: 25px;
}
#pano {
  z-index: 0;
  height: 100vh;
}
</style>
