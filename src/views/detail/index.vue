<template>
  <div>
    <headerBox></headerBox>
    <div class="body">
      <div class="fbox w1200">
        <div class="top">
          <topSwiper
            :list="list"
            :index="courseProgressIndex.mouduleId"
            @change="toTopIndex"
          ></topSwiper>
        </div>
        <div class="mid">
          <div class="left">
            <div class="leftBox">
              <xgVideo
                ref="player"
                :url="courseInfo.videoUrl"
                :isFirstLearn="isFirstLearn"
                @change="videoChange"
                @playStart="playStart"
                @playEnd="playEnd"
              ></xgVideo>
            </div>
          </div>
          <div class="right">
            <directory
              :list="rightList"
              :cIndex="courseProgressIndex.courseId"
              :sIndex="courseProgressIndex.sectionId"
              :isToping="isToping"
              @change="directoryChange"
            ></directory>
          </div>
        </div>
      </div>
    </div>
    <footerBox></footerBox>
    <videoRecording
      :isShow="isShowVideoRecording"
      @complete="videoRecordingComplete"
    ></videoRecording>
    <faceRecognition
      :isShow="isShowFaceRecognition"
      @complete="faceRecognitionComplete"
    ></faceRecognition>
  </div>
</template>

<script>
import headerBox from "@/components/header";
import footerBox from "@/components/footer";
import xgVideo from "@/components/video";
import topSwiper from "@/components/topSwiper";
import directory from "@/components/directory";
import videoRecording from "@/components/videoRecording";
import faceRecognition from "@/components/faceRecognition";

export default {
  name: "detail",
  components: {
    headerBox,
    footerBox,
    xgVideo,
    topSwiper,
    directory,
    videoRecording,
    faceRecognition,
  },
  watch: {
    topIndex: {
      handler(val) {
        if (this.list[val] && this.list[val].children) {
          this.rightList = this.list[val].children;
          if (val == this.courseProgressIndex.mouduleId) {
            this.isToping = true;
            this.isFirstLearn = true;
            this.courseInfo =
              this.rightList[this.courseProgressIndex.courseId].children[
                this.courseProgressIndex.sectionId
              ];
          } else {
            this.isToping = false;
            this.isFirstLearn = false;
            this.courseInfo = this.rightList[0].children[0];
          }
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
      isShowFaceRecognition: false,
      courseProgress: {},
      courseProgressIndex: {
        mouduleId: 0,
        courseId: 0,
        sectionId: 0,
      },
      isToping: false,
      isFirstLearn: true,
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
          this.getCourseProgress();
        }
      });
    },
    getCourseProgress() {
      let courseProgress = {};
      if (this.name == "gyb") {
        courseProgress = {
          mouduleId: "gyb-1",
          courseId: "gyb-1-2",
          sectionId: "gyb-1-2-4",
        };
      } else if (this.name == "syb") {
        courseProgress = {
          mouduleId: "sybExtend-6",
          courseId: "sybExtend-6-1",
          sectionId: "sybExtend-6-1-1",
        };
      } else if (this.name == "netCreation") {
        courseProgress = {
          mouduleId: "projectSection",
          courseId: "projectSection4",
          sectionId: "projectSection4-2",
        };
      }
      this.courseProgress = courseProgress;
      for (let a = 0; a < this.list.length; a++) {
        if (this.list[a].mouduleId == this.courseProgress.mouduleId) {
          this.courseProgressIndex.mouduleId = a;
          this.topIndex = a;
          for (let b = 0; b < this.list[a].children.length; b++) {
            if (
              this.list[a].children[b].courseId == this.courseProgress.courseId
            ) {
              this.courseProgressIndex.courseId = b;
              for (
                let c = 0;
                c < this.list[a].children[b].children.length;
                c++
              ) {
                if (
                  this.list[a].children[b].children[c].sectionId ==
                  this.courseProgress.sectionId
                ) {
                  this.courseProgressIndex.sectionId = c;
                }
              }
            }
          }
        }
      }
    },
    toTopIndex(index) {
      this.topIndex = index;
    },
    directoryChange(e, isFirstLearn) {
      this.isFirstLearn = isFirstLearn;
      this.courseInfo = e;
    },
    videoChange(ctime) {
      if (ctime == 3&&this.isFirstLearn) {
        this.$refs.player.pause();
        this.isShowVideoRecording = true;
      }
      if (ctime == 6&&this.isFirstLearn) {
        this.$refs.player.pause();
        this.isShowFaceRecognition = true;
      }
      console.log(999, ctime);
    },
    videoRecordingComplete(file) {
      console.log(file);
      this.isShowVideoRecording = false;
      this.$refs.player.play();
    },
    playStart(e) {
      console.log("kaishi", e);
    },
    playEnd(e) {
      console.log("jieshu", e);
    },
    faceRecognitionComplete() {
      console.log("人脸成功");
      this.isShowFaceRecognition = false;
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
