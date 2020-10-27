<template>
  <view>
    <!-- 主页背景 -->
    <image
      id="user-bg"
      src="/static/user-bg.jpg"
      :style="{ width: screenWidth + 'px' }"
    ></image>

    <!-- TODO:字体颜色调整 -->
    <cu-custom class="text-white" :isBack="true"
      ><block slot="backText">返回</block
      ><block slot="content">我的主页</block></cu-custom
    >

    <!-- 个人信息栏 -->
    <!-- 动态列表 -->
    <scroll-view scroll-y>
      <view
        class="cu-list menu card-menu margin-top-sm margin-bottom-xl shadow-lg radius"
      >
        <view class="cu-item"> </view>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      // 屏幕宽高
      screenHeight: 0,
      screenWidth: 0,
    };
  },
  onLoad() {
    this.setUserBgSize();
  },
  methods: {
    setUserBgSize() {
      var that = this;
      uni.getSystemInfo({
        success: (res) => {
          that.screenHeight = res.windowHeight;
          that.screenWidth = res.windowWidth;
        },
      });
    },
  },
};
</script>

<style scoped>
#user-bg {
  z-index: -1;
  position: relative;
}
</style>