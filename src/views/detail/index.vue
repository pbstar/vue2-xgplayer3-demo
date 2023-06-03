<template>
  <div>
    <headerBox></headerBox>
    <div class="body">
      <div class="fbox w1200">
        <div class="top">
          <topSwiper
            :list="list"
            :index="topIndex"
            @change="toTopIndex"
          ></topSwiper>
        </div>
        <div class="mid">
          <div class="left">
            <div class="leftBox">
              <xgVideo
                ref="player"
                :url="courseInfo.videoUrl"
                @change="videoChange"
              ></xgVideo>
            </div>
          </div>
          <div class="right">
            <directory :list="rightList" @change="directoryChange"></directory>
          </div>
        </div>
      </div>
    </div>
    <footerBox></footerBox>
    <videoRecording
      :isShow="isShowVideoRecording"
      @complete="videoRecordingComplete"
    ></videoRecording>
  </div>
</template>

<script>
import headerBox from "@/components/header";
import footerBox from "@/components/footer";
import xgVideo from "@/components/video";
import topSwiper from "@/components/topSwiper";
import directory from "@/components/directory";
import videoRecording from "@/components/videoRecording";

export default {
  name: "detail",
  components: {
    headerBox,
    footerBox,
    xgVideo,
    topSwiper,
    directory,
    videoRecording,
  },
  watch: {
    topIndex: {
      handler(val) {
        if (this.list[val] && this.list[val].children) {
          this.rightList = this.list[val].children;
          console.log(this.rightList);
          this.courseInfo = this.rightList[0].children[0];
        }
      },
      immediate: true,
    },
  },
  data() {
    return {
      name: "",
      list: [],
      topIndex: -1,
      rightList: [],
      courseInfo: "",
      isShowVideoRecording: false,
    };
  },
  created() {
    if (!this.$route.query.name) {
      this.$notify({
        title: "提示",
        message: "未匹配到课程！",
        duration: 3000,
        type: "warning",
        position: "bottom-right",
        dangerouslyUseHTMLString: true,
      });
      this.$router.push("list");
      return;
    }
    this.name = this.$route.query.name;
    this.getList();
  },
  beforeDestroy() {
    if (this.isShowVideoRecording) this.isShowVideoRecording = false;
  },
  methods: {
    getList() {
      this.$http.getCourseList(this.name).then((res) => {
        if (res.code == 200 && res.data.length > 0) {
          this.list = res.data;
          this.topIndex = 0;
        }
      });
    },
    getCourseProgress() {
      let courseProgress = {};
    },
    toTopIndex(index) {
      this.topIndex = index;
    },
    directoryChange(e) {
      this.courseInfo = e;
    },
    videoChange(ctime) {
      if (ctime == 3) {
        this.$refs.player.pause();
        this.isShowVideoRecording = true;
      }
      console.log(999, ctime);
    },
    videoRecordingComplete(file) {
      console.log(file);
      this.isShowVideoRecording = false;
      this.$refs.player.play();
    },
  },
};
</script>
<style lang="scss" scoped>
.body {
  min-height: calc(100vh - 450px);
  background: #f8fafb;
  .fbox {
    overflow: hidden;
    .top {
      margin-top: 10px;
      margin-bottom: 5px;
    }
    .mid {
      height: 498px;
      display: flex;
      justify-content: space-between;
      margin-bottom: 50px;
      margin-top: 10px;
      .left {
        width: 862px;
        height: 498px;
        background: #f1f1f1;
        padding: 15px;
        border-radius: 14px;
        box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.15);
        .leftBox {
          width: 100%;
          height: 100%;
          background-color: #ffffffa5;
        }
      }
      .right {
        width: 300px;
        height: 498px;
      }
    }
  }
}
</style>
