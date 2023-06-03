<template>
  <div>
    <div class="fbox">
      <div class="top"></div>
      <div class="mid">
        <div class="title">
          <h2>华普亿方在线培训系统</h2>
        </div>
        <div class="mids w1200">
          <div class="m1">
            <h3>寄语：</h3>
            <p>
              学会让自己安静，把思维沉浸下来，慢慢降低对事物的欲望。把自我经常归零，每天都是新的起点，没有年龄的限制，只要你对事物的欲望适当的降低，会赢得更多的求胜机会。
            </p>
          </div>
          <div class="m2">
            <p>平台联系人：010-52831025</p>
            <p>（工作日 09:00-18:00）</p>
            <p>平台链接：https://hbjg.newchuangye.com/</p>
            <p>北京华普亿方科技集团股份有限公司版权所有</p>
          </div>
          <div class="m3">
            <img
              src="https://hbjg.newchuangye.com/online/images/erweima.png"
              alt=""
            />
            <p>(移动端扫码下载)</p>
          </div>
        </div>
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
  height: 88px;
  background: url("@/assets/imgs/footer-top.png") #fff no-repeat top center;
  background-size: 100% 100%;
}
.mid {
  height: 272px;
  width: 1920px;
  background: url("@/assets/imgs/footer-mid.png") repeat-y;
  position: relative;

  .title {
    color: #fff;
    font-size: 18px;
    width: 100%;
    text-align: center;
    position: absolute;
    top: -25px;
    letter-spacing: 3px;
  }
  .mids {
    height: 220px;
    margin-top: 30px;
    background-color: #fff;
    z-index: 100;
    display: flex;
    padding: 30px 40px 35px;
    justify-content: space-between;
    align-items: flex-end;
    .m1,
    .m2 {
      width: 350px;
      color: #3e2ddc;
      line-height: 30px;
      h3 {
        font-size: 20px;
        margin-bottom: 30px;
      }
      p {
        font-size: 14px;
      }
    }
    .m1 {
      width: 450px;
      p {
        text-indent: 2em;
      }
    }
    .m3 {
      width: 180px;
      display: flex;
      align-items: center;
      flex-direction: column;
      img {
        width: 120px;
        height: 120px;
      }
      p {
        font-size: 14px;
        line-height: 30px;
      }
    }
  }
}
</style>
