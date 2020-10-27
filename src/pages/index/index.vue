<template>
  <view class="bg-gradual-cyan">
    <view class="DrawerPage" :class="listDrawerShow == true ? 'show' : ''">
      <!-- 背景地图 -->
      <image
        id="bg-map"
        src="/static/map.png"
        :style="{ width: screenWidth + 'px', height: screenHeight + 'px' }"
      ></image>
      <!-- 顶部菜单按钮 -->
      <view class="padding" style="position: absolute">
        <button
          class="cu-btn cuIcon bg-cyan shadow shadow-blur"
          @tap="listDrawerShow = true"
        >
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
          class="shadow"
          color="#39B54A"
        ></switch>
        <button class="cu-btn cuIcon bg-cyan lg shadow shadow-blur">
          <text class="text-xsl cuIcon-add"></text>
        </button>
        <button
          class="cu-btn cuIcon bg-cyan shadow shadow-blur"
          @tap="sendQrModal = true"
        >
          <text class="cuIcon-qr_code"></text>
        </button>
      </view>
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

    <!-- 左侧抽屉菜单栏 -->
    <!-- TODO:左侧菜单栏抽出的时候，右侧缩不回去。。。 -->
    <view
      class="DrawerClose"
      :class="listDrawerShow == true ? 'show' : ''"
      @tap="listDrawerShow = false"
    >
      <text class="cuIcon-pullright"></text>
    </view>

    <scroll-view
      scroll-y
      class="DrawerWindow"
      :class="listDrawerShow == true ? 'show' : ''"
    >
      <!-- 头像栏 -->
      <view class="text-center">
        <view
          class="cu-avatar xl round shadow-warp"
          style="
            background-image: url(https://ossweb-img.qq.com/images/lol/web201310/skin/big99008.jpg);
          "
        ></view>
        <!-- TODO: 头像栏这里还要再修改 -->
        <view class="margin-top">
          <text class="text-black"> 怀念在地球的日子 </text>
        </view>
      </view>

      <!-- 下方列表 -->
      <view
        class="cu-list menu card-menu margin-top-xl margin-bottom-xl shadow-lg"
      >
        <navigator
          class="cu-item arrow"
          v-for="(item, index) in leftListItems"
          :key="index"
          NavigateTo
          :url="'/pages/' + item.toPage + '/index'"
        >
          <view class="content">
            <text
              :class="'cuIcon-' + item.icon + ' ' + 'text-' + item.color"
            ></text>
            <text class="text-grey">{{ item.content }}</text>
          </view>
        </navigator>
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
      // 是否开启接受周围人的二维码
      acceptQrcode: false,
      // 询问是否开启二维码的模态框
      acQrModal: false,
      // 询问是否向周围发码的模态框
      sendQrModal: false,
      // 左侧菜单栏伸缩状态
      listDrawerShow: false,
      // 左侧菜单栏内内容
      leftListItems: [
        {
          id: 1,
          content: "名片墙",
          toPage: "socialCards",
          icon: "card",
          color: "pink",
        },
        {
          id: 2,
          content: "收到的码",
          toPage: "recivedQrcodes",
          icon: "qr_code",
          color: "brown",
        },
        {
          id: 3,
          content: "个人主页",
          toPage: "homePage",
          icon: "homefill",
          color: "orange",
        },
      ],
    };
  },
  onLoad() {
    this.setMapSize();
  },
  methods: {
    setMapSize() {
      var that = this;
      uni.getSystemInfo({
        success: (res) => {
          that.screenHeight = res.windowHeight;
          that.screenWidth = res.windowWidth;
        },
      });
    },
    showAcQrModal() {
      if (!this.acceptQrcode) {
        // 显示同意收码对话框
        this.acceptQrcode = true;
        this.acQrModal = true;
      } else {
        // TODO:这里可能要再做一个对话
        this.acceptQrcode = false;
      }
    },
    acQrOn() {
      this.acceptQrcode = true;
      this.acQrModal = false;
    },
    // TODO:状态切换有大问题！！！！！！！！！！！！
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

/* 以下样式为左侧菜单栏 */
page {
  /* background-image: var(--gradualGreen); */
  /* #1cbbb4, */
  background-image: linear-gradient(45deg, #1cbbb4, #178b86);
  width: 100vw;
  /* height: 100vw; */
  position: absolute;
  overflow: hidden;
}

.DrawerPage {
  position: fixed;
  width: 100vw;
  height: 100vh;
  left: 0vw;
  background-color: #f1f1f1;
  transition: all 0.4s;
}

.DrawerPage.show {
  transform: scale(0.9, 0.9);
  left: 85vw;
  box-shadow: 0 0 60upx rgba(0, 0, 0, 0.2);
  transform-origin: 0;
}

.DrawerWindow {
  position: absolute;
  width: 85vw;
  height: 100vh;
  left: 0;
  top: 0;
  transform: scale(0.9, 0.9) translateX(-100%);
  opacity: 0;
  pointer-events: none;
  transition: all 0.4s;
  padding: 100upx 0;
}

.DrawerWindow.show {
  transform: scale(1, 1) translateX(0%);
  opacity: 1;
  pointer-events: all;
}

.DrawerClose {
  position: absolute;
  width: 40vw;
  height: 100vh;
  right: 0;
  top: 0;
  color: transparent;
  padding-bottom: 30upx;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  background-image: linear-gradient(
    90deg,
    rgba(0, 0, 0, 0.01),
    rgba(0, 0, 0, 0.6)
  );
  letter-spacing: 5px;
  font-size: 50upx;
  opacity: 0;
  pointer-events: none;
  transition: all 0.4s;
}

.DrawerClose.show {
  opacity: 1;
  pointer-events: all;
  width: 15vw;
  color: #fff;
}

.DrawerPage .cu-bar.tabbar .action button.cuIcon {
  width: 64upx;
  height: 64upx;
  line-height: 64upx;
  margin: 0;
  display: inline-block;
}

.DrawerPage .cu-bar.tabbar .action .cu-avatar {
  margin: 0;
}

.DrawerPage .nav {
  flex: 1;
}

.DrawerPage .nav .cu-item.cur {
  border-bottom: 0;
  position: relative;
}

.DrawerPage .nav .cu-item.cur::after {
  content: "";
  width: 10upx;
  height: 10upx;
  background-color: currentColor;
  position: absolute;
  bottom: 10upx;
  border-radius: 10upx;
  left: 0;
  right: 0;
  margin: auto;
}

.DrawerPage .cu-bar.tabbar .action {
  flex: initial;
}
/* 以上样式为左侧菜单栏 */
</style>
