<template>
  <div style="width: 100%; height: 100%">
    <div class="fbox">
      <div class="abox" v-for="(item, index) in list" :key="index">
        <div
          class="atop"
          :class="'title-bg' + (index % 5)"
          @click="toAtop(item, index)"
        >
          {{ item.courseTitle }}
        </div>
        <div class="amid" v-show="expansionIndex == index">
          <div
            class="bbox"
            :class="indexs % 2 != 0 ? 'bbox2n' : ''"
            v-for="(items, indexs) in item.children"
            :key="indexs + 's'"
            @click="toCourse(items)"
          >
            <div class="bbtype">{{ items.sectionInfo }}</div>
            <div class="bbtitle overflowDot1">{{ items.sectionTitle }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "directory",
  props: {
    list: {
      type: Array,
      default: [],
    },
  },
  data() {
    return {
      expansionIndex: 0,
    };
  },
  methods: {
    toAtop(item, index) {
      if (this.expansionIndex == index) {
        this.expansionIndex = -1;
      } else {
        this.expansionIndex = index;
      }
    },
    toCourse(e) {
      this.$emit("change", e);
    },
  },
};
</script>
<style lang="scss" scoped>
.fbox {
  width: 100%;
  height: 100%;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  .abox {
    flex-shrink: 0;
    width: 285px;
    background: #fff;
    box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.2);
    border-radius: 8px;
    margin: 3px 2px 7px;
    overflow: hidden;
    .atop {
      width: 100%;
      height: 50px;
      cursor: pointer;
      color: #fff;
      display: flex;
      align-items: center;
      padding: 0 10px;
    }
    .title-bg0 {
      background: linear-gradient(to right, #03d9af, #7de8b7);
    }

    .title-bg1 {
      background: linear-gradient(to right, #22a1f5, #82c5f3);
    }

    .title-bg2 {
      background: linear-gradient(to right, #745df8, #9e8dff);
    }

    .title-bg3 {
      background: linear-gradient(to right, #b05bfa, #ca91fc);
    }

    .title-bg4 {
      background: linear-gradient(to right, #f35ac7, #f78fd9);
    }
    .amid {
      padding: 6px 10px 8px 6px;
      .bbox {
        width: 100%;
        height: 35px;
        display: flex;
        align-items: center;
        cursor: pointer;
        .bbtype {
          margin-left: 5px;
          margin-right: 10px;
          display: inline-block;
          width: 38px;
          height: 20px;
          text-align: center;
          background-color: #57b57d;
          font-size: 14px;
          color: #ffffff;
          border-radius: 5px;
          overflow: hidden;
        }
        .bbtitle {
          font-size: 14px;
          width: 160px;
        }
      }
      .bbox2n {
        background-color: #fafafa;
      }
    }
  }
}
</style>
