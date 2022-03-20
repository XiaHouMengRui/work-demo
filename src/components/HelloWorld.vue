<template>
  <div>
    <div class="navbar" v-if="isShowTabs">
      <div
        v-for="(item, index) in menuList"
        :key="index"
        class="tab"
        :class="{ active: tabActive === index }"
        @click="handleToggleTab(item, index)"
      >
        {{ item }}
      </div>
    </div>

    <div class="page-header" ref="header-ref">这是头部</div>

    <div class="page-content">
      <div
        v-for="(item, index) in menuList"
        :key="index"
        :ref="item"
        class="content"
      >
        <div style="height: 50px"></div>
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menuList: ["tab1", "tab2", "tab3", "tab4"],
      tabActive: 0,
      isShowTabs: false,
    };
  },
  mounted() {
    window.onscroll = this.debounce(() => {
      this.handleScrollEvent();
    }, 15);
  },
  methods: {
    // 监听滚动事件
    handleScrollEvent() {
      // 当该元素距离顶部的高度小于1时,将navbar展示出来
      const t1 = this.$refs["header-ref"].getBoundingClientRect().top;
      this.isShowTabs = t1 < 1 ? true : false;
      this.menuList.forEach((item, index) => {
        // 当该元素距离顶部的高度小于1时,将对应的tab添加选中效果
        const t2 = this.$refs[item][0].getBoundingClientRect().top;
        if (t2 < 1) {
          this.tabActive = index;
        }
      });
    },
    // 防抖函数
    debounce(fn, delay) {
      let timer = null;
      return function () {
        clearTimeout(timer);

        timer = setTimeout(() => {
          fn.apply(this, arguments);
          clearTimeout(timer);
        }, delay);
      };
    },
    // tab点击事件
    handleToggleTab(dom, index) {
      this.tabActive = index;
      this.$refs[dom][0].scrollIntoView({ block: "start", behavior: "smooth" });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.page-header {
  height: 50px;
  line-height: 50px;
  text-align: center;
}
.navbar {
  width: 100%;
  height: 50px;
  line-height: 50px;
  padding: 0 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0px 3px 13px 0px rgba(0, 0, 0, 0.17);
  position: sticky;
  top: 0;
  left: 0;
  z-index: 99;
  background-color: #fff;

  .tab {
    flex: 1;
    text-align: center;
    cursor: pointer;

    &.active {
      color: red;
    }
  }
}

.page-content {
  .content {
    height: 100vh;
    padding: 0 30px;
  }
}
</style>
