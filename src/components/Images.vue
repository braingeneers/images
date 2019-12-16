<template>
  <div>
    <template v-if="manifest.captures.length > 0">
      <div>Images for {{ uuid }}</div>
      <!-- Hard code as we can't determine from manifest row and column -->
      <table>
        <tr v-for="row in 4" :key="row">
          <td v-for="col in 6" :key="row * 10 + col">
            <img v-on:click="onClick" class="capture" @error="missing" v-bind:data-row="row" v-bind:data-col="col"
                 :src="`${endpoint}/${uuid}/images/${manifest.captures[0]}/cameraB${row}${col}/${0 + 1}.jpg`"/>
          </td>
        </tr>
      </table>
    </template>
    <template v-if="manifest.captures.length > 0">
      <div>Row: {{ curRow }} Col: {{ curCol }}</div>
      <div>T: {{ manifest.captures[curTimestampIndex] }} Z: {{ curZ }}</div>
      <img v-pan="onPan" class="capture" @error="missing"
           :src="`${endpoint}/${uuid}/images/${manifest.captures[curTimestampIndex]}/cameraB${curRow}${curCol}/${curZ + 1}.jpg`"/>
    </template>
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
        captures: []
      },
      curTimestampIndex: 0,
      curZ: 0, 
      curRow: 1,
      curCol: 1,
      startTimestampIndex: 0,
      startZ: 0,
      panning: false
    }
  },
  methods: {
    load: function() {
      console.log("Loading...", this.uuid)
      console.log(`${this.endpoint}/${this.uuid}/images/manifest.json`)
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
    missing(event) {
      console.log("Missing image", event.target.src)
      event.target.src = require("../assets/missing.jpg")
    },
    onClick(event) {
      this.curRow = event.target.dataset.row
      this.curCol = event.target.dataset.col
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
            this.startTimestampIndex + event.deltaX / 25,
            0), this.manifest.captures.length - 1))
      } else if (event.direction == 8 || event.direction == 16) {
        this.curZ = Math.round( 
          Math.min(Math.max(
            this.startZ + event.deltaY / 25,
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
