<template>
  <div>
    <div class="fbox">
      <div class="sbox w1200">
        <div class="left">
          <img src="@/assets/imgs/header-logo.png" alt="" />
        </div>
      </div>
      <!-- <div class="f2box">
        <div class="s2box w1200">
          <div class="left"></div>
          <div class="right"></div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  name: "headerBox",
  data() {
    return {};
  },
  methods: {},
};
</script>
<style lang="scss" scoped>
.fbox {
  background: #fff;
  width: 100%;
  height: 60px;
  line-height: 60px;
  box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.1);
  z-index: 1000;

  .sbox {
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .left {
      height: 48px;
      width: 300px;
    }
  }
  .f2box {
    overflow: hidden;
  }
  .s2box {
    position: relative;
    z-index: auto;
    height: calc(100vh - 60px);

    .left {
      position: absolute;
      // left: -360px;
      left: -446px;
      width: 446px;
      height: 657px;
      background-image: url("@/assets/imgs/left-flower.png");
    }
    .right {
      position: absolute;
      right: -729px;
      width: 729px;
      height: 866px;
      background-image: url("@/assets/imgs/right-flower.png");
    }
  }
}
</style>
