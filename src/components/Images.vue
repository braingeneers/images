<template>
  <div>
    <div>Images for {{ uuid }}</div>
    <div>{{ manifest.timestamp }}</div>
    <ul id="timestamp-list">
      <li v-for="t in manifest.captures" v-bind:key="t">
        {{ t }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ["uuid"],
  data: function() {
    return {
      manifest: {}
    }
  },
  methods: {
    load: function() {
      console.log("Loading...", this.uuid)
      const base = "https://s3.nautilus.optiputer.net/braingeneers"
      fetch(`${base}/rcurrie/archive/${this.uuid}/images/manifest.json`)
        .then(stream => stream.json())
        .then(data => this.manifest = data)
        .catch(error => {
          console.log(error)
          alert("Unable to load experiment, does the uuid exist?")
        })
    }
  },
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
