<template>
  <div v-on:click="$emit('reset')" class="btn absolute"></div>
</template>
<script>
export default {
  name: 'ReturnToStart',
};
</script>

<style>
body {
  margin: 0px;
}
.btn {
  height: 5rem;
  width: 5rem;
  background-color: white;
  bottom: 0;
  left: 0;
  margin: 20px;
  border-radius: 50%;
}
.absolute {
  position: absolute;
}
</style>
