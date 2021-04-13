<template>
  <div id="app">
    <img alt="Braingeneers Imaging" src="./assets/logo.png">
    <label>uuid</label>
    <input v-model="uuid" placeholder="Ryan-4-2020-02-04">
    <label>group identifier</label>
    <input v-model="groupID" placeholder="C">
    <button v-on:click="$refs.images.load()">Load</button>
    <button v-on:click="$refs.images2.load()">Load Begining</button>
    <Images ref="images" v-bind:groupID="groupID" v-bind:uuid="uuid" v-bind:endpoint="endpoint"/>
    <Images2 ref="images2" v-bind:groupID="groupID" v-bind:uuid="uuid" v-bind:endpoint="endpoint"/>
  </div>
</template>

<script>
import Images from "./components/Images.vue"
import Images2 from "./components/Images2.vue"

export default {
  name: "app",
  components: {
    Images,
    Images2
  },
  data: function () {
    return {
      uuid: "Ryan-4-2020-02-04",
      groupID: "C",
      endpoint: "https://s3.nautilus.optiputer.net/braingeneers/imaging"
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
