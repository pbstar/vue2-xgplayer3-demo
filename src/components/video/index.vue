<template>
  <div style="width: 100%; height: 100%" v-loading="loading">
    <div class="xgplayer" ref="xgplayer"></div>
  </div>
</template>

<script>
import Player from "xgplayer";
import { Events } from "xgplayer";
import "xgplayer/dist/index.min.css";
export default {
  name: "xgVideo",
  props: {
    url: "",
    isprogress: false,
    isplaybackrate: false,
  },
  watch: {
    url: {
      handler(val) {
        if (val) {
          this.getStart();
        }
      },
      immediate: true,
    },
  },
  data() {
    return {
      player: null,
      loading: false,
    };
  },
  mounted() {},
  beforeDestroy() {
    if (this.player != null) {
      this.player.destroy();
      this.player = null;
    }
  },
  methods: {
    getStart() {
      this.loading = true;
      if (this.player != null) {
        this.player.destroy();
        this.player = null;
      }
      let ignores = ["cssfullscreen"];
      if (!this.isprogress) ignores.push("progress");
      if (!this.isplaybackrate) ignores.push("playbackrate");
      this.player = new Player({
        el: this.$refs.xgplayer,
        url: this.url,
        poster: "./static/imgs/hopefound-video-pic.jpg",
        height: "100%",
        width: "100%",
        ignores,
      });
      this.player.on(Events.LOADED_DATA, (e) => {
        let time1 = setTimeout(() => {
          this.loading = false;
          clearTimeout(time1);
        }, 300);
      });
      let currentTime = 0;
      let ctime = 0;
      this.player.on(Events.TIME_UPDATE, (e) => {
        ctime = Math.round(e.currentTime);
        if (ctime == currentTime) return;
        else currentTime = ctime;
        this.$emit("change", ctime);
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.xgplayer {
  width: 100%;
  height: 100%;
}
</style>
