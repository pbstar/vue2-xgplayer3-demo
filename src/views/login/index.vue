<template>
  <div>
    <headerBox></headerBox>
    <div class="body">
      <div class="fbox">
        <div class="sbox w1200">
          <div></div>
          <div class="loginBox">
            <h3 class="title">后台管理系统</h3>
            <el-input
              class="username"
              placeholder="请输入账号"
              prefix-icon="el-icon-user"
              v-model="username"
            >
            </el-input>
            <el-input
              class="password"
              placeholder="请输入密码"
              :show-password="true"
              type="password"
              prefix-icon="el-icon-lock"
              v-model="password"
            >
            </el-input>
            <el-button
              class="btn"
              type="primary"
              @click="toLogin"
              :loading="isLoading"
              >登 录</el-button
            >
          </div>
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
  name: "login",
  components: {
    headerBox,
    footerBox,
  },
  data() {
    return {
      username: "测试",
      password: "123",
      isLoading: false,
    };
  },
  methods: {
    toLogin() {
      if (!this.username) {
        this.$message({
          message: "请输入任意账号密码即可登录",
          type: "warning",
        });
        return;
      }
      if (!this.password) {
        this.$message({
          message: "请输入密码",
          type: "warning",
        });
        return;
      }
      this.isLoading = true;
      this.$unit.setLocalStorage("username", this.username);
      this.$router.push({
        name: "home",
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.body {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;

  .fbox {
    height: 601px;
    width: 1920px;
    background-image: url("@/assets/imgs/banner-login.jpg");
    .sbox {
      height: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .loginBox {
        border-radius: 20px;
        padding: 0px 30px;
        width: 350px;
        height: 420px;
        background-color: #ffffffef;

        .title {
          margin-bottom: 20px;
          font-size: 26px;
        }
        .username {
          margin-bottom: 10px;
          /deep/ .el-input__inner {
            height: 45px;
            line-height: 45px;
          }
        }
        .password {
          margin-bottom: 15px;
          /deep/ .el-input__inner {
            height: 45px;
            line-height: 45px;
          }
        }
        .btn {
          width: 100%;
          margin-bottom: 50px;
        }
      }
    }
  }
}
</style>
