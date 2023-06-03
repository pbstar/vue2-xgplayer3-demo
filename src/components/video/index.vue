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
    url: {
      type: String,
      default: "",
    },
    isprogress: {
      type: Boolean,
      default: false,
    },
    isplaybackrate: {
      type: Boolean,
      default: false,
    },
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
      this.$nextTick(() => {
        let ignores = getIgnores(this);
        let startTime = getStartTime(this);

        this.player = new Player({
          el: this.$refs.xgplayer,
          url: this.url,
          poster: "./static/imgs/hopefound-video-pic.jpg",
          height: "100%",
          width: "100%",
          ignores,
          startTime,
        });
        this.loadedData();
        this.timeUpdate();
      });
      function getStartTime(that) {
        let video_url = that.$unit.getLocalStorage("video_url");
        let video_progress = that.$unit.getLocalStorage("video_progress");
        let startTime = 0;
        if (video_url == that.url) {
          startTime = video_progress;
        } else {
          that.$unit.removeLocalStorage("video_progress");
        }
        return startTime;
      }
      function getIgnores(that) {
        let ignores = ["cssfullscreen"];
        if (!that.isprogress) ignores.push("progress");
        if (!that.isplaybackrate) ignores.push("playbackrate");
        return ignores;
      }
    },
    play() {
      if (this.player) {
        this.player.play();
      }
    },
    pause() {
      if (this.player) {
        this.player.pause();
      }
    },
    loadedData() {
      this.player.on(Events.LOADED_DATA, (e) => {
        this.$unit.setLocalStorage("video_url", this.url);
        let time1 = setTimeout(() => {
          this.loading = false;
          clearTimeout(time1);
        }, 300);
      });
    },
    timeUpdate() {
      let currentTime = 0;
      let ctime = 0;
      this.player.on(Events.TIME_UPDATE, (e) => {
        if (e.paused) return;
        ctime = Math.round(e.currentTime);
        if (ctime == currentTime) return;
        else currentTime = ctime;
        this.$unit.setLocalStorage("video_progress", ctime);
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
