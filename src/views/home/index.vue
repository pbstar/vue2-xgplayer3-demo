<template>
  <div>
    <headerBox></headerBox>
    <div class="body">
      <div class="bbox w1200">
        <div class="welcome"></div>
        <div class="pc">
          <h2>欢迎登入</h2>
          <h2>在线培训系统！</h2>
          <a href="javascript:;" @click="toList">开始学习</a>
        </div>
      </div>
    </div>
    <footerBox></footerBox>
  </div>
</template>

<script>
import headerBox from "@/components/header";
import footerBox from "@/components/footer";
export default {
  name: "home",
  components: {
    headerBox,
    footerBox,
  },
  data() {
    return {};
  },
  methods: {
    toList() {
      this.$router.push("list");
    },
  },
};
</script>
<style lang="scss" scoped>
.body {
  min-height: calc(100vh - 450px);
  background: #f8fafb;
  .bbox {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    .welcome {
      width: 475px;
      height: 543px;
      margin: 30px 50px;
      background-image: url("@/assets/imgs/welcome.png");
      background-size: cover;
    }
    .pc {
      width: 465px;
      height: 439px;
      margin: 30px 50px;
      background-image: url("@/assets/imgs/macbook.png");
      background-size: cover;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-top: 80px;
      h2 {
        height: 50px;
        font-size: 34px;
        color: #447fe5;
      }
      a {
        font-size: 20px;
        text-align: center;
        width: 160px;
        height: 46px;
        line-height: 46px;
        border-radius: 30px;
        color: #fff;
        background: linear-gradient(45deg, #394af7, #8864f8);
        margin-top: 100px;
      }
    }
  }
}
</style>
