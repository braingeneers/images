<template>
  <div id="app">
    <img alt="Braingeneers Imaging" src="./assets/logo.png">
    <label>uuid</label>
    <input v-model="uuid" placeholder="Incubator-test-11-08">
    <button v-on:click="$refs.images.load()">Load</button>
    <Images ref="images" v-bind:uuid="uuid" v-bind:endpoint="endpoint"/>
  </div>
</template>

<script>
import Images from "./components/Images.vue"

export default {
  name: "app",
  components: {
    Images
  },
  data: function () {
    return {
      uuid: "Incubator-test-11-08",
      endpoint: "https://s3.nautilus.optiputer.net/braingeneers/archive"
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
