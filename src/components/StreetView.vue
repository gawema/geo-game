<template>
  <div id="map" class="minimap-container"></div>
  <div id="pano"></div>
  <ReturnToStart v-on:reset="resetLocation" />
</template>

<script>
// import google from "googleapis";
import locations from './../assets/locations';
import ReturnToStart from './ReturnToStart.vue';

export default {
  name: 'StreetView',
  components: {
    ReturnToStart,
  },
  data() {
    return {
      //   googleApiKey: process.env.GOOGLE_API_KEY,
      randCoord: locations[Math.floor(Math.random() * locations.length)],
      map: null,
      guessCoords: null,
      currentPin: null,
      streetView: {},
      startLocation: '',
    };
  },
  mounted() {
    const split = this.randCoord.split(',');
    let lati = Number(split[0]);
    let longi = Number(split[1]);
    this.coords = { lat: lati, lng: longi };
    window.checkAndAttachMapScript(this.createMap);
    window.addEventListener('load', () => {
      document
        .querySelector(
          '[title="Open this area in Google Maps (opens a new window)"]'
        )
        .remove();
      var cheatElements = document.getElementsByClassName('gm-style-cc');
      while (cheatElements.length > 0) {
        cheatElements[0].parentNode.removeChild(cheatElements[0]);
      }
    });
  },
  methods: {
    createMap() {
      // initialize a location in streetview

      this.map = new window.google.maps.Map(document.getElementById('map'), {
        center: new window.google.maps.LatLng(0, 0),
        zoom: 1,
      });
      this.streetView = new window.google.maps.StreetViewPanorama(
        document.getElementById('pano'),
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

      this.map.addListener('click', (mapsMouseEvent) => {
        this.guessCoords = mapsMouseEvent.latLng.toJSON();
        this.placeMarker(this.guessCoords);
        console.log(this.checkCoordDistance());
      });

      this.startLocation = new window.google.maps.LatLng({
        lat: this.streetView.position.lat(),
        lng: this.streetView.position.lng(),
      });
    },
    checkCoordDistance() {
      const R = 6371e3; // metres
      const φ1 = (this.coords.lat * Math.PI) / 180; // φ, λ in radians
      const φ2 = (this.guessCoords.lat * Math.PI) / 180;
      const Δφ = ((this.guessCoords.lat - this.coords.lat) * Math.PI) / 180;
      const Δλ = ((this.guessCoords.lng - this.coords.lng) * Math.PI) / 180;

      const a =
        Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
        Math.cos(φ1) * Math.cos(φ2) * Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

      const d = R * c; // in metres
      return d;
    },
    resetLocation() {
      this.streetView.setPosition(this.startLocation);
    },
    placeMarker(coords) {
      if (this.currentPin != null) {
        this.currentPin.setMap(null);
      }
      this.currentPin = new window.google.maps.Marker({
        position: new window.google.maps.LatLng(coords.lat, coords.lng),
        map: this.map,
      });
      this.currentPin.setMap(this.map);
    },
  },
};
</script>
<style scoped>
.minimap-container {
  height: 30%;
  width: 40%;
  position: absolute;
  bottom: 0;
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
