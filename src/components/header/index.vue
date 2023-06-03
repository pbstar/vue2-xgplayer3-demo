<template>
  <div>
    <div class="fbox">
      <div class="sbox w1200">
        <div class="left">
          <img src="@/assets/imgs/header-logo.png" alt="" />
        </div>
        <div class="right">
          <el-dropdown trigger="click" v-if="username" @command="handleCommand">
            <span class="el-dropdown-link">
              {{ username }}<i class="el-icon-arrow-down el-icon--right"></i>
            </span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="1">我的课程</el-dropdown-item>
              <el-dropdown-item command="2">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
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
    return {
      username: "",
    };
  },
  created() {
    let username = this.$unit.getLocalStorage("username");
    if (username) {
      this.username = username;
    }
  },
  methods: {
    handleCommand(e) {
      if (e == 1) {
        this.$router.push("list");
      } else if (e == 2) {
        this.$confirm("确认退出登录吗？")
          .then(() => {
            this.$unit.clearLocalStorage();
            this.$router.push("login");
          })
          .catch((err) => {
            console.log(err);
          });
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.fbox {
  background: #fff;
  width: 100%;
  height: 60px;
  border-bottom: 1px solid #eeeeee89;

  .sbox {
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .left {
      height: 48px;
      width: 300px;
    }
    .right {
      .el-dropdown-link {
        cursor: pointer;
      }
    }
  }
  // .f2box {
  //   overflow: hidden;
  // }
  // .s2box {
  //   position: relative;
  //   z-index: auto;
  //   height: calc(100vh - 60px);

  //   .left {
  //     position: absolute;
  //     // left: -360px;
  //     left: -446px;
  //     width: 446px;
  //     height: 657px;
  //     background-image: url("@/assets/imgs/left-flower.png");
  //   }
  //   .right {
  //     position: absolute;
  //     right: -729px;
  //     width: 729px;
  //     height: 866px;
  //     background-image: url("@/assets/imgs/right-flower.png");
  //   }
  // }
}
</style>
