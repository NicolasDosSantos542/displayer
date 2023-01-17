<script >
import axios from "axios";

export default ({
  setup() {

  },
  data() {
    return {
      width: window.innerWidth * 70 / 100,
      video: {
        src: "",
        name: "Aucune vidéo en cours"
      }
    }
  },
  async created() {
    await this.getDisplayedVideo()
  },
  async updated(){
   

  },
  methods: {
    async getDisplayedVideo() {
      try {
        const res = await axios.get(`http://localhost:3001/displayed`);
        if (res.data.src) {
          this.video = res.data;
        }
      } catch (e) {
        console.error(e);
      }
    },
    async onEnd(){
      console.log('test on Ended', this.$refs["videoPlayer"])
      await this.getDisplayedVideo()
      this.$refs["videoPlayer"].play()
    }
  },

})
</script>

<template>

  <div id="displayer">
    <video :key="video.src" :width="width" id="videoPlayer" ref="videoPlayer" autoplay @ended="onEnd()">
      <source v-bind:src="video.src + '.mov'" type="video/mov">
      <source v-bind:src="video.src + '.mp4'" type="video/mp4">

    </video>
    <div><h2>{{ video.name }}</h2></div>


  </div>
</template>

<style>
#displayer {
  margin-left: auto;
  margin-right: auto;
  padding: 15px;
}
</style>