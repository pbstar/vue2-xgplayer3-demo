<template>
  <div>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div
          class="topItem swiper-slide"
          :class="'topItem' + (ind % 5)"
          v-for="(item, ind) in list"
          :key="ind"
          @click="toItem(ind)"
        >
          <span class="overflowDot2">{{ item.mouduleTitle }}</span>
          <img
            v-show="ind > index"
            src="@/assets/imgs/detail/lock.png"
            alt=""
          />
          <img
            v-show="ind <= index"
            src="@/assets/imgs/detail/lock-open.png"
            alt=""
          />
        </div>
      </div>
      <div class="swiper-button-prev"></div>
      <div class="swiper-button-next"></div>
    </div>
  </div>
</template>

<script>
//导入js
import Swiper from "swiper";
//引入css
import "swiper/css/swiper.min.css";
export default {
  name: "topSwiper",
  props: {
    list: {
      type: Array,
      default: [],
    },
    index: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {};
  },
  mounted() {
    this.getSwiper();
  },
  methods: {
    getSwiper() {
      new Swiper(".swiper-container", {
        updateOnImagesReady: true,
        slidesPerView: 9,
        observer: true,
        observeParents: true,
        observeSlideChildren: true,
        navigation: {
          nextEl: ".swiper-button-next", // 右按钮
          prevEl: ".swiper-button-prev", // 左按钮
        },
      });
    },
    toItem(index) {
      if (index > this.index) {
        this.$message({
          message:
            "请先完成课程：《" + this.list[this.index].mouduleTitle + "》！",
          type: "warning",
        });
        return;
      }
      this.$emit("change", index);
    },
  },
};
</script>
<style lang="scss" scoped>
.swiper-button-prev:after,
.swiper-button-next:after {
  font-size: 25px;
  font-weight: bold;
  // color: #fff;
}

.swiper-wrapper {
  display: flex;
  // flex-wrap: wrap;
  // overflow-x: scroll;
  padding-left: 30px;
  // padding-right: 50px;
  .topItem {
    width: 124px !important;
    height: 66px;
    background-size: 100% 100%;
    color: #fff;
    font-size: 14px;
    padding: 3px 43px 15px 12px;
    text-align: center;
    display: flex;
    align-items: center;
    flex-shrink: 0;
    position: relative;
    cursor: pointer;
    margin: 5px 2px 0;
    span {
      width: 100%;
    }
    img {
      width: 30px;
      height: 30px;
      position: absolute;
      right: 13px;
    }
  }
  .topItem0 {
    background-image: url("@/assets/imgs/detail/m1-bg.png");
  }
  .topItem1 {
    background-image: url("@/assets/imgs/detail/m2-bg.png");
  }
  .topItem2 {
    background-image: url("@/assets/imgs/detail/m3-bg.png");
  }
  .topItem3 {
    background-image: url("@/assets/imgs/detail/m4-bg.png");
  }
  .topItem4 {
    background-image: url("@/assets/imgs/detail/m5-bg.png");
  }
}
</style>
