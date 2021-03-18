<template>
  <div id="street-view">
      {{ this.randCoord }}
  </div>
</template>

<script>
    import google from 'googleapis'
    import locations from './../assets/locations'

export default {
       
    name: "StreetView",
    data(){
        return {
            googleApiKey: process.env.GOOGLE_API_KEY,
            randCoord: locations[Math.floor(Math.random() * locations.length)],
        }
    },
    methods: {
        initialize() {
            console.log(locations.locations);
            new google.maps.StreetViewPanorama(
            document.getElementById("street-view"),
            {
                position: { lat: this.randCoord[0], lng: this.randCoord[1] },
                pov: { heading: 165, pitch: 0 },
                zoom: 1,
                addressControl: false,
            }
        );
    }
    }
}
</script>
