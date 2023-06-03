<template>
  <div>
    <el-dialog
      title="音视频录制"
      class="box"
      :visible.sync="isShow"
      :show-close="false"
      :close-on-click-modal="false"
      width="404px"
    >
      <div v-loading="loading">
        <video ref="video"></video>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "videoRecording",
  props: {
    isShow: {
      type: Boolean,
      default: false,
    },
    isDownLoad: {
      type: Boolean,
      default: false,
    },
    time: {
      type: Number,
      default: 5,
    },
  },
  watch: {
    isShow: {
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
      loading: false,
    };
  },
  beforeDestroy() {
    this.stop();
  },
  mounted() {},
  methods: {
    getStart() {
      this.loading = true;
      if (navigator.mediaDevices === undefined) {
        navigator.mediaDevices = {};
      }
      navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: true,
        })
        .then((stream) => {
          this.loading = false;
          this.mediaStreamTrack =
            typeof stream.stop === "function" ? stream : stream.getTracks()[0];
          this.video_stream = stream;
          this.$refs.video.srcObject = stream;
          this.$refs.video.play();
          this.record();
          var time1 = setTimeout(() => {
            this.stop();
            clearTimeout(time1);
          }, this.time * 1000);
        })
        .catch((err) => {
          this.loading = false;
          console.log(err);
          alert("调用摄像头失败！");
        });
    },
    record() {
      let mediaRecorder;
      let options;
      this.recordedBlobs = [];
      if (typeof MediaRecorder.isTypeSupported === "function") {
        // 根据浏览器来设置编码参数
        if (MediaRecorder.isTypeSupported("video/webm;codecs=vp9")) {
          options = {
            MimeType: "video/webm;codecs=h264",
          };
        } else if (MediaRecorder.isTypeSupported("video/webm;codecs=h264")) {
          options = {
            MimeType: "video/webm;codecs=h264",
          };
        } else if (MediaRecorder.isTypeSupported("video/webm;codecs=vp8")) {
          options = {
            MimeType: "video/webm;codecs=vp8",
          };
        }
        mediaRecorder = new MediaRecorder(this.video_stream, options);
      } else {
        console.log("当前不支持isTypeSupported，使用浏览器的默认编解码器");
        mediaRecorder = new MediaRecorder(this.video_stream);
      }
      mediaRecorder.start();
      // 视频录制监听事件
      mediaRecorder.ondataavailable = (e) => {
        // 录制的视频数据有效
        if (e.data && e.data.size > 0) {
          this.recordedBlobs.push(e.data);
        }
      };
      // 停止录像后增加下载视频功能，将视频流转为mp4格式
      mediaRecorder.onstop = () => {
        if (this.recordedBlobs.length == 0) {
          alert("录制视频无效或清晰度过低，请重试！");
          return;
        }
        const blob = new Blob(this.recordedBlobs, { type: "video/mp4" });
        this.recordedBlobs = [];
        let timestamp = new Date().getTime();
        let filename = timestamp + "_" + this.$unit.getRandomStr(4) + ".mp4";
        let file = new File([blob], filename, {
          type: "video/mp4",
          lastModified: timestamp,
        });

        if (this.isDownLoad) {
          const videoUrl = window.URL.createObjectURL(blob);
          let a = document.createElement("a");
          a.href = videoUrl;
          a.download = filename;
          a.click();
        }
        this.$emit("complete", file);
      };
    },
    // 停止录制
    stop() {
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
  width: 384px;
  height: 216px;
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
  height: 236px;
}
</style>
