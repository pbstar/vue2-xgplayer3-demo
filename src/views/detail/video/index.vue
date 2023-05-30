<template>
  <div>
    <div class="xgplayer" ref="xgplayer"></div>
  </div>
</template>

<script>
import Player from "xgplayer";
import "xgplayer/dist/index.min.css";
export default {
  name: "detailVideo",
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
        url: "//abc.com/**/*.mp4",
        height: "100%",
        width: "100%",
      });
    },
  },
};
</script>
<style lang="scss" scoped>
</style>
