<template>
  <div>
    <div class="xgplayer" ref="xgplayer"></div>
  </div>
</template>

<script>
import Player from "xgplayer";
import "xgplayer/dist/index.min.css";
export default {
  name: "xgVideo",
  data() {
    return {
      player: null,
    };
  },
  mounted() {
    this.getStart();
  },
  methods: {
    getStart() {
      if (this.player != null) {
        this.player.destroy();
        this.player = null;
      }
      this.player = new Player({
        el: this.$refs.xgplayer,
        url: "https://bsy.newchuangye.com/onlineEducationTangshan/video/64-hebeiGYB24/1.mp4",
        height: "100%",
        width: "100%",
      });
    },
  },
};
</script>
<style lang="scss" scoped>
</style>
