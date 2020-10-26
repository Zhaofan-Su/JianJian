<template>
  <view>
    <!-- 背景地图 -->
    <image
      id="bg-map"
      src="/static/map.png"
      :style="{ width: screenWidth + 'px', height: screenHeight + 'px' }"
    ></image>
    <!-- 顶部菜单按钮 -->
    <view class="padding" style="position: absolute">
      <button class="cu-btn cuIcon bg-cyan">
        <text class="xslg cuIcon-list"></text>
      </button>
    </view>

    <!-- 底部三个按钮 -->
    <view
      id="bottom-bar"
      class="padding flex flex-wrap justify-between align-center"
      :style="{ top: screenHeight * 0.9 + 'px' }"
    >
      <switch
        @change="showAcQrModal"
        :class="acceptQrcode ? 'checked' : ''"
        color="#39B54A"
      ></switch>
      <button class="cu-btn cuIcon bg-cyan lg">
        <text class="text-xsl cuIcon-add"></text>
      </button>
      <button class="cu-btn cuIcon bg-cyan" @tap="sendQrModal = true">
        <text class="cuIcon-qr_code"></text>
      </button>
    </view>

    <!-- 开启位置权限模态框 -->
    <view class="cu-modal" :class="acQrModal == true ? 'show' : ''">
      <view class="cu-dialog">
        <view class="cu-bar bg-white justify-end">
          <view class="content">共享位置吗？</view>
          <view class="action" @tap="acQrModal = false">
            <text class="cuIcon-close text-red"></text>
          </view>
        </view>
        <view class="padding-xl">
          开启位置共享之后可以收到周围的人发来的二维码哦！
        </view>
        <view class="cu-bar bg-white justify-end">
          <view class="action">
            <button class="cu-btn line-green text-green" @tap="acQrOff">
              取消
            </button>
            <button class="cu-btn bg-green margin-left" @tap="acQrOn">
              确定
            </button>
          </view>
        </view>
      </view>
    </view>

    <!-- 送码模态框 -->
    <view class="cu-modal" :class="sendQrModal == true ? 'show' : ''">
      <view class="cu-dialog">
        <view class="cu-bar bg-white justify-end">
          <view class="content">给周围的人送码？</view>
          <view class="action" @tap="sendQrModal = false">
            <text class="cuIcon-close text-red"></text>
          </view>
        </view>
        <view class="padding-xl"> 这是你的二维码 </view>
        <view class="cu-bar bg-white justify-end">
          <view class="action">
            <button
              class="cu-btn line-green text-green"
              @tap="sendQrModal = false"
            >
              取消
            </button>
            <button class="cu-btn bg-green margin-left" @tap="sendQrCode">
              确定
            </button>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      screenHeight: 0,
      screenWidth: 0,
      acceptQrcode: false,
      acQrModal: false,
      sendQrModal: false,
    };
  },
  onLoad() {
    this.setMapSize();
  },
  methods: {
    setMapSize() {
      var that = this;
      uni.getSystemInfo({
        success: function (res) {
          that.screenHeight = res.windowHeight;
          that.screenWidth = res.windowWidth;
        },
      });
    },
    showAcQrModal() {
      if (!this.acceptQrcode) {
        // 显示同意收码对话框
        this.acQrModal = true;
      } else {
        // TODO:这里可能要再做一个对话框
        this.acceptQrcode = false;
      }
    },
    acQrOn() {
      this.acceptQrcode = true;
      this.acQrModal = false;
    },
    acQrOff() {
      // TODO:状态转换有问题，5555
      this.acceptQrcode = false;
      this.acQrModal = false;
    },
    sendQrCode() {
      // TODO:发送请求

      //   uni.request({
      //     url: "",
      //     data: {
      //       text: "uni.request",
      //     },
      //     success: (res) => {
      //       uni.showToast({
      //         title: "送码成功！",
      //         mask: true,
      //         duration: 1500,
      //       });
      //     },
      //     fail: (res) => {
      //       uni.showToast({
      //         title: "网络有问题呀，再试试吧",
      //         mask: true,
      //         icon: "none",
      //         duration: 1500,
      //       });
      //     },
      //   });

      // 送码成功提醒
      // 先把外层的modal弄掉
      this.sendQrModal = false;
      uni.showToast({
        title: "送码成功！",
        mask: true,
        duration: 1500,
        fail: (res) => {
          console.log("接口调用失败：" + res);
        },
      });
    },
  },
};
</script>

<style>
#bg-map {
  z-index: -1;
  position: absolute;
}

#bottom-bar {
  position: relative;
}
</style>
