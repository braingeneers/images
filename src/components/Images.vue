<template>
  <div>
    <div>Images for {{ uuid }}</div>
    <div>T: {{ manifest.captures[curTimestampIndex] }} Z: {{ curZ }}</div>
    <ul id="url-list">
      <li v-for="s in manifest.num_stacks" :key="s">
        <img v-pan="onPan" 
             :src="`${endpoint}/${uuid}/images/${manifest.captures[curTimestampIndex]}/${s-1}/${curZ}.jpg`"/>
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
      manifest: {
        num_stacks: 0,
        captures: [""]
      },
      curTimestampIndex: 0,
      curZ: 0, 
      startTimestampIndex: 0,
      startZ: 0,
      panning: false
    }
  },
  methods: {
    load: function() {
      console.log("Loading...", this.uuid)
      fetch(`${this.endpoint}/${this.uuid}/images/manifest.json`)
        .then(stream => stream.json())
        .then(data => {
          this.manifest = data
        })
        .catch(error => {
          console.log(error)
          alert("Unable to load experiment, does the uuid exist?")
        })
    },
    onPan(event) {
      // 0 = none, 2 = left, 4 = right, 8 = up, 16 = down,
      if (!this.panning) {
        this.panning = true
        this.startTimestampIndex = this.curTimestampIndex
        this.startZ = this.curZ
      } else if (event.isFinal) {
        this.panning = false
      } else if (event.direction == 2 || event.direction == 4) {
        this.curTimestampIndex = Math.round( 
          Math.min(Math.max(
            this.startTimestampIndex + event.deltaX / 50,
            0), this.manifest.captures.length - 1))
      } else if (event.direction == 8 || event.direction == 16) {
        this.curZ = Math.round( 
          Math.min(Math.max(
            this.startZ + event.deltaY / 50,
            0), this.manifest.stack_size - 1))
      }
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
