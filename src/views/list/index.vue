<template>
  <div>
    <headerBox></headerBox>
    <div class="body">
      <div class="fbox w1200">
        <div
          class="sbox"
          v-for="(item, index) in list"
          :key="index"
          @click="toDetail(item)"
        >
          <img
            v-show="item.name == 'gyb'"
            src="@/assets/imgs/list/index-course-2.png"
            :alt="item.name"
          />
          <img
            v-show="item.name == 'syb'"
            src="@/assets/imgs/list/index-course-4.png"
            :alt="item.name"
          />
          <img
            v-show="item.name == 'netCreation'"
            src="@/assets/imgs/list/index-course-6.png"
            :alt="item.name"
          />
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
  name: "list",
  components: {
    headerBox,
    footerBox,
  },
  data() {
    return {
      list: [
        {
          name: "gyb",
        },
        {
          name: "syb",
        },
        {
          name: "netCreation",
        },
      ],
    };
  },
  methods: {
    toDetail(e) {
      this.$router.push({
        name: "detail",
        query: {
          name: e.name,
        },
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.body {
  min-height: calc(100vh - 450px);
  background: #f8fafb;
  .fbox {
    display: flex;
    align-items: flex-end;
    flex-wrap: wrap;
    padding: 30px 0 50px;
    .sbox {
      width: 180px;
      margin: 20px;
      cursor: pointer;
      img {
        width: 100%;
      }
    }
  }
}
</style>
