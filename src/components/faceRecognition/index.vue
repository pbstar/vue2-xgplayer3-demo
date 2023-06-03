<template>
  <div>
    <el-dialog
      title="人脸识别"
      class="box"
      :visible.sync="isShow"
      :show-close="false"
      :close-on-click-modal="false"
      width="420px"
    >
      <div v-loading="loading" class="fbox">
        <video ref="video" id="video" autoplay></video>
        <canvas id="canvas" width="400" height="300"></canvas>
        <canvas
          ref="canvas"
          style="display: none"
          width="400"
          height="300"
        ></canvas>
      </div>
    </el-dialog>
  </div>
</template>

<script>
require("tracking/build/tracking-min.js");
require("tracking/build/data/face-min.js");
// require("tracking/build/data/mouth-min.js");
// require("tracking/examples/assets/stats.min.js");
export default {
  name: "faceRecognition",
  props: {
    isShow: {
      type: Boolean,
      default: false,
    },
    isDownLoad: {
      type: Boolean,
      default: false,
    },
  },
  watch: {
    isShow: {
      handler(val) {
        if (val) {
          this.getStart();
          this.myInterval = setInterval(() => {
            this.$nextTick(() => {
              this.setImage();
            });
          }, 2000);
        } else {
          if (this.myInterval) {
            this.stop();
          }
        }
      },
      immediate: true,
    },
  },
  data() {
    return {
      loading: false,
      isHasFace: false,
      trackerTask: "",
    };
  },
  mounted() {},
  beforeDestroy() {
    this.stop();
  },
  methods: {
    getStart() {
      this.loading = true;
      if (navigator.mediaDevices === undefined) {
        navigator.mediaDevices = {};
      }
      navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: false,
        })
        .then((stream) => {
          this.loading = false;
          if (!this.$refs.video) return;
          this.$refs.video.srcObject = stream;
          this.$refs.video.play();
          this.checkFace();
        })
        .catch((err) => {
          this.loading = false;
          console.log(err);
          alert("调用摄像头失败！");
        });
    },
    checkFace() {
      var canvas = document.getElementById("canvas");
      var context = canvas.getContext("2d");

      this.tracker = new tracking.ObjectTracker("face");
      this.tracker.setInitialScale(4);
      this.tracker.setStepSize(2);
      this.tracker.setEdgesDensity(0.1);
      this.trackerTask = tracking.track("#video", this.tracker, {
        camera: true,
      });
      this.tracker.on("track", (event) => {
        if (event.data.length <= 0) {
          this.isHasFace = false;
          return;
        }
        this.isHasFace = true;
        context.clearRect(0, 0, canvas.width, canvas.height);
        event.data.forEach(function (rect) {
          context.strokeStyle = "#a64ceb";
          context.strokeRect(rect.x, rect.y, rect.width, rect.height);
        });
      });
    },
    setImage() {
      if (!this.isHasFace) {
        return false;
      }
      clearInterval(this.myInterval);
      let ctx = this.$refs["canvas"].getContext("2d");
      // 把当前视频帧内容渲染到canvas上
      ctx.drawImage(this.$refs["video"], 0, 0, 400, 300);
      // 转base64格式、图片格式转换、图片质量压缩---支持两种格式image/jpeg+image/png
      let imgBase64 = this.$refs["canvas"].toDataURL("image/jpeg");
      // console.log(imgBase64);
      if (this.isDownLoad) {
        let ADOM = document.createElement("a");
        ADOM.href = imgBase64;
        ADOM.download = new Date().getTime() + ".jpeg";
        ADOM.click();
      }
      this.$emit("complete");
    },
    stop() {
      if (this.myInterval) {
        clearInterval(this.myInterval);
        this.trackerTask.stop();
      }
      if (!this.$refs.video) return;
      if (!this.$refs.video.srcObject) return;
      const stream = this.$refs.video.srcObject;
      const tracks = stream.getTracks();
      tracks.forEach((track) => {
        track.stop();
      });
    },
  },
};
</script>
<style lang="scss" scoped>
video {
  width: 400px;
  height: 300px;
  background-color: #000;
}
.box /deep/ .el-dialog__header {
  padding: 10px;
  background-color: #eee;
}
.box /deep/ .el-dialog__title {
  font-size: 14px;
}
.box /deep/ .el-dialog__body {
  padding: 10px;
  height: 320px;
  // height: 452px;
}
.fbox {
  position: relative;
}
#canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
