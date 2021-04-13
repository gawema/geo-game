<template>
    <div class="zoom-out">
        <span>zoom out</span>
    </div>
</template>

<script>
export default {
    name: "MiniMapFunctions",
    data(){
        return{

        }
    },
    mounted(){
        document.getElementsByClassName('zoom-out')[0].addEventListener('click', () => {
            this.mapZoomOut()
        });
    },
    methods: {
        mapZoomOut(){
            console.log('OOF')
            // document.getElementById("map").setZoom(12);
        }
    }
}
</script>

<style scoped>
    .zoom-out{
        background-color: red;
        z-index: 2;
        position: absolute;
        top: 0;
        right: 0;
        width: 2.5em;
        height: 2.5em;
    }
</style>