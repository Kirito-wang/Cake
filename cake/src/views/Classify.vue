<template>
  <div>
    <div class="mySearch">
      <!-- 点击搜索框后要跳到一个新的组件页面 -->
      <mt-search autofocus v-model="value" :result="filterResult"></mt-search>
    </div>
    <!-- <h1>dsadsa</h1> -->
    <!-- 左侧边栏 -->
    <div class="let-tabbar">
      <div class="left-panel">
        <cube-scroll>
          <cube-tab-bar
            v-model="selectedLabel"
            :data="left_list.name"
            @change="changeHandler"
          ></cube-tab-bar>
        </cube-scroll>
      </div>
    </div>
    <!-- 右边 -->
    <div class="right-panel">
      <cube-scroll ref="scroll">
        <ul>
          <li v-for="(elem,i) in right_list" :key="i">
            <img :src="elem.pic" />
            <span v-text="elem.name"></span>
          </li>
        </ul>
      </cube-scroll>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: "",
      // 默认数据(搜索的关键字)
      defaultResult: ["蛋糕", "123", "1", "124567"],
      left_list: {},
      right_list: [],
      selectedLabel: "蛋糕"
    };
  },
  created() {
    // 加载第一个区域
    this.right_list = this.left_list[this.selectedLabel];
    // 请求数据
    this.axios.get("product/classify").then(result => {
      console.log(result.data.msg);
      this.left_list = result.data.msg;
      console.log(Object.keys(this.left_list).map(label => ({label})))
    });
  },
  methods: {
    changeHandler(label) {
      // 点击左边后换到别的区域
      this.right_list = this.left_list[label];
    }
  },
  computed: {
    // 显示搜索的内容
    filterResult() {
      return this.defaultResult.filter(value =>
        new RegExp(this.value, "i").test(value)
      );
    }
  },
  watch: {
    selectedLabel(newV) {
      // console.log(newV);
    }
  }
};
</script>
<style lang="stylus" rel="stylesheet/stylus">
/* 搜索框的高 */
.mySearch {
  height: 52px;
}

.mint-searchbar {
  background-color: #efeff4 !important;
}

/* 取消的文字样式 */
.mySearch .mint-searchbar-cancel {
  font-size: 14px !important;
  color: #000 !important;
}

/* 搜索框的文字样式 */
.mySearch .mint-searchbar-core {
  font-size: 14px !important;
}

/* 左侧导航 */
.let-tabbar {
  position: absolute;
  top: 52px;
  left: 0;
  bottom: 0;
  width: 100px;
  background-color: #f7f7f7;
}

.cube-scroll-list-wrapper {
  .cube-tab-bar {
    flex-wrap: wrap;

    .cube-tab {
      width: 100%;
      flex-basis: unset;
      height: 40px;
      line-height: 40px;
      font-size: 14px;
      color: #555;

      &.cube-tab_active {
        color: #fe9170;

        &::after {
          content: '';
          display: block;
          border: 1px solid #fe9170;
        }
      }
    }
  }
}

.right-panel {
  position: absolute;
  top: 52px;
  left: 100px;
  right: 0;
  bottom: 0;
  background-color: #fff;

  ul {
    display: flex;
    /* 每个项目的间隔相等 */
    justify-content: space-around;
    flex-wrap: wrap;
    margin: 20px 10px 0 10px;
  }

  li {
    height: 100px;
    align-items: center;
    text-align: center;
    background-color: #fff;
    width: 28%;
    margin-bottom: 30px;

    img {
      display: block;
      width: 100%;
    }

    span {
      font-size: 16px;
      color: #555;
    }
  }
}
</style>


