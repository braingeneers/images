<template>
  <div>
    <div>Images for {{ uuid }}</div>
    <div>T: {{ curTimestamp }} Z: {{ curZ }}</div>
    <ul id="timestamp-list">
      <li v-for="t in manifest.captures" :key="t">
        <button v-on:click="(event) => {curTimestamp = t}">Set new value</button>
        {{ t }}
      </li>
    </ul>
    <ul id="z-list">
      <li v-for="z in manifest.stack_size" :key="z">
        {{ z }}
      </li>
    </ul>
    <ul id="url-list">
      <li v-for="s in manifest.num_stacks" :key="s">
        <img :src="`${endpoint}/${uuid}/images/${curTimestamp}/${s-1}/${curZ}.jpg`" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Images",
  props: ["uuid", "endpoint"],
  data: function() {
    return {
      manifest: {},
      curTimestamp: "",
      curZ: 0 
    }
  },
  methods: {
    load: function() {
      console.log("Loading...", this.uuid)
      fetch(`${this.endpoint}/${this.uuid}/images/manifest.json`)
        .then(stream => stream.json())
        .then(data => {
          this.manifest = data
          this.curTimestamp = this.manifest.captures[0]
          console.log(this.curTimestamp)
        })
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
