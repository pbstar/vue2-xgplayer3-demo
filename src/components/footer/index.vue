<template>
  <div>
    <div class="fbox">
      <div class="top"></div>
      <div class="mid">
        <div class="mids w1200"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "footerBox",
  data() {
    return {};
  },
  methods: {},
};
</script>
<style lang="scss" scoped>
.fbox {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  padding-top: 20px;
  z-index: 1001;
}
.top {
  width: 1920px;
  height: 98px;
  background: url("@/assets/imgs/footer-top.png") #fff no-repeat top center;
  background-size: 100% 100%;
}
.mid {
  height: 272px;
  width: 1920px;
  background: url("@/assets/imgs/footer-mid.png") repeat-y;
  .mids {
    height: 220px;
    margin-top: 30px;
    background-color: #fff;
    z-index: 100;
  }
}
</style>
