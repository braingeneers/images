<template>
  <div>
    <div>Images for {{ uuid }}</div>
    <div>T: {{ manifest.captures[curTimestampIndex] }} Z: {{ curZ }}</div>
    <!-- Hard code as we can't determine from manifest row and column -->
    <ul v-for="row in 4" :key="row">
      <li v-for="col in 6" :key="row * 10 + col">
        <img v-pan="onPan" class="capture"
             :src="`${endpoint}/${uuid}/images/${manifest.captures[curTimestampIndex]}/cameraB${row}${col}/${curZ + 1}.jpg`"/>
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
        stack_size: 0,
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

.capture {
  width: 100%;
}
</style>
