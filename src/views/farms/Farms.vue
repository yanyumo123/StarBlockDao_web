<template>
  <div class="back">
    <list class="contantBack">
      <!-- <div class="contantList"> -->
      <!-- 头部 -->
      <div class="topBackBox">
        <img :src="topBackImgUrl" class="topImg" />
        <div class="topBackContant">
          <p class="topTitle">FARMS</p>
          <p class="topSubTitle">Stake NFTs to Earn</p>
          <div class="topItemDataSuperBox">
            <div class="itemDataBox">
              <p class="itemDataBox_topText">12653.872 ETH</p>
              <p class="itemDataBox_bottomText">{{ $t("farms.topItem1") }}</p>
            </div>
            <div class="vSepLine"></div>
            <div class="itemDataBox">
              <p class="itemDataBox_topText">1243</p>
              <p class="itemDataBox_bottomText">{{ $t("farms.topItem2") }}</p>
            </div>
            <div class="vSepLine"></div>
            <div class="itemDataBox">
              <p class="itemDataBox_topText">12653 STB</p>
              <p class="itemDataBox_bottomText">{{ $t("farms.topItem3") }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Farms -->
      <div class="itemsBox">
        <farmitem :items="items"></farmitem>
      </div>
    </list>

    <div class="bottomDesBox">
      <img src="@/assets/img/farms/farmsDes.png" class="farmsDesImg" />
      <div class="farmsDesTextBox">
        <p class="farmsDesText">
          <span>{{ "· " + $t("farms.tip1") }}</span>
          <br />
          <span>{{ "· " + $t("farms.tip2") }}</span>
          <br />
          <span>{{ "· " + $t("farms.tip3") }}</span>
          <br />
          <span>{{ "· " + $t("farms.tip4") }}</span>
          <br />
          <span>{{ "· " + $t("farms.tip5") }}</span>
          <br />
          <span>{{ "· " + $t("farms.tip6") }}</span>
        </p>
      </div>
    </div>

    <el-dialog
      title=""
      :visible.sync="actionAlertShow"
      :width="elDialogEditSellDataWidth"
      :height="elDialogEditSellDataHeight"
      :close-on-click-modal="false"
      :show-close="false"
      :fullscreen="false"
      :destroy-on-close="true"
      center
      top="70px"
      :lock-scroll="false"
      :close-on-press-escape="false"
    >
      <div class="alertContantBox1">
        <div class=""></div>
        <p class="alertTitle">合集NFT</p>
        <img
          class="closeAlertIcon"
          src="@/assets/img/farms/optionViewClose.svg"
          @click="closeAlertAction"
        />
      </div>

      <div class="alertSectionBox1">
        <div class="itemDataBox">
          <p class="itemDataBox_topText_alert">12653.872 ETH</p>
          <p class="itemDataBox_bottomText_alert">TVL</p>
        </div>
        <div class="vSepLine_alert"></div>
        <div class="itemDataBox">
          <p class="itemDataBox_topText_alert">12653.872 ETH</p>
          <p class="itemDataBox_bottomText_alert">TVL</p>
        </div>
        <div class="vSepLine_alert"></div>
        <div class="itemDataBox">
          <p class="itemDataBox_topText_alert">12653.872 ETH</p>
          <p class="itemDataBox_bottomText_alert">TVL</p>
        </div>
      </div>

      <div class="alertSectionBox2">
        <div class="switchBtnBox">
          <button :class="isSwitch1 ? 'switchBtn_on' : 'switchBtn_off'" @click="switchBtn(1)">
            抵押
          </button>
          <button :class="!isSwitch1 ? 'switchBtn_on' : 'switchBtn_off'" @click="switchBtn(2)">
            已抵押
          </button>
        </div>
        <button class="unPledgeAction">解抵押 3个</button>
      </div>

      <div class="itemsSuperBox">
        <div class="alertSectionBox3">
          <div class="alertSectionBox3_textBox1">
            <p class="alertSectionBox3_text1">NFT</p>
          </div>
          <div class="alertSectionBox3_textBox2">
            <p class="alertSectionBox3_text2">详细信息</p>
          </div>
          <div class="alertSectionBox3_textBox3">
            <p class="alertSectionBox3_text3">操作</p>
          </div>
        </div>
        <div class="selectItemSepLine"></div>
        <!-- <div v-for="(item, index) in selectItems"> -->
        <selectnft :items="selectItems"></selectnft>
        <!-- </div> -->
      </div>

      <p class="alertTip1">· 抵押或解压后将自动领取当前奖励</p>
      <p class="alertTip2">· 根据您出游WrappedNFT可解抵押对应的NFT</p>
    </el-dialog>
  </div>
</template>

<script>
import { onConnect, initWeb3Modal, resetApp } from "@/common/useWallet";
import Farmitem from "../farms/children/FarmsItem.vue";
import Selectnft from "../farms/children/SelectNFT.vue";

import { List } from "vant";
export default {
  name: "Farms",
  created() {},
  components: {
    Farmitem,
    Selectnft
  },

  data() {
    var topImgHeight = 0;
    topImgHeight = document.documentElement.clientWidth > 750 ? "7rem" : "6rem";

    return {
      topImgHeight: topImgHeight,
      topBackImgUrl:
        document.documentElement.clientWidth > 750
          ? require("@/assets/img/farms/topBack.png")
          : require("@/assets/img/farms/mobile/topBack.png"),
      isSwitch1: true,
      selectItems: [
        { select: false },
        { select: false },
        { select: false },
        { select: false },
        { select: false },
        { select: false }
      ],
      elDialogEditSellDataWidth: document.documentElement.clientWidth > 750 ? "900px" : "350px",
      elDialogEditSellDataHeight: "918px",
      actionAlertShow: true,
      items: ["1", "2", "3", "4", "5"],
      isScroll: false,
      isClickTab: false,
      active: 0,
      isShowMobile: document.documentElement.clientWidth > 750 ? false : true
    };
  },

  beforeRouteEnter(to, from, next) {
    undefined;
    next(vm => {
      undefined;
      // 通过 `vm` 访问组件实例,将值传入fromPath
    });
  },
  beforeUpdate() {},
  beforeRouteEnter(to, from, next) {
    undefined;
    next(vm => {
      undefined;
      // 通过 `vm` 访问组件实例,将值传入fromPath
      // vm.setScrollToPostion();
    });
  },
  created() {
    // resetApp();
    // this.accountsChange();

    this.$nextTick(() => {
      // console.log("this.$route.path*******",this.$route.path);
      // this.setScrollToPostion();
    });
    // this.ratio = this.detectZoom();

    if (this.ratio > 100) {
      this.metaItemContantDesFontSize = 14 / bili + "px";
    }
    // this.startCountTime();
  },
  watch: {
    windowWidth(val) {
      this.windowWidth = val;
      let that = this;
      if (that.windowWidth > 750) {
        // that.isShowMobile = false;
      } else {
        // that.isShowMobile = true;
      }
      console.log(that.windowWidth);
      // this.ratio = this.detectZoom();

      var html = document.documentElement; //获取到html元素
      var hWidth = that.windowWidth; //获取到html的宽度
      // 当hWidth大于640时，则物理分辨率小于1280（这就看设备的devicePixelRatio这个值了），应该去访问pc网站了
      // alert(hWidth);
      var bili = Number(this.ratio) / 100;
      if (this.ratio > 100) {
        this.metaItemContantDesFontSize = 14 / (3 * bili) + "px";
      }
    }
  },
  mounted() {
    this.$bus.$on("alertAction", val => {
      if (val == "1") {
        this.actionAlertShow = true;
      }
    });
    var that = this;
    // <!--把window.onresize事件挂在到mounted函数上-->
    window.onresize = () => {
      return (() => {
        window.fullHeight = document.documentElement.clientHeight;
        window.fullWidth = document.documentElement.clientWidth;
        // that.windowHeight = window.fullHeight; // 高
        that.windowWidth = window.fullWidth; // 宽
      })();
    };
  },

  methods: {
    switchBtn(index) {
      this.isSwitch1 = index == 1 ? true : false;
    },
    closeAlertAction() {
      this.actionAlertShow = false;
    },
    resetApp() {
      // alert(resetApp);
      resetApp();
    },
    async accountsChange() {
      const provider = await initWeb3Modal().connect();
      provider.on("accountsChanged", function (accounts) {
        alert(accounts[0]);
      });
    },
    handleWalletConnect() {
      onConnect();
      // if (connected) {
      //   console.log("afterConnectdWallet", connected);
      // }
    }
  }
};
</script>

<style scoped>
.back {
  display: flex;
  flex-direction: column;
  /* height: 100%; */
  width: 100%;
  background-color: #f7faff;
  /* background-color: #ea4ae0; */
}

.contantBack {
  /* background-color: #ea4ae0; */
  align-items: center;
  margin-top: 3.5rem;
  z-index: 100;
  display: flex;
  flex-direction: column;
  /* height: 100%; */
  width: 95%;
  margin-left: 2.5%;
  overflow-x: hidden;
}

.contantList {
  align-items: center;
  /* margin-top: 3.1rem; */
  z-index: 100;
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 95%;
  margin-left: 2.5%;
  overflow-x: hidden;
}

.topBackBox {
  position: relative;
}

.topImg {
  width: 100%;
  height: 6rem;
}
.topBackContant {
  top: 0%;
  left: 0%;
  height: 100%;
  position: absolute;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.topTitle {
  margin-top: 0.4rem;
  font-size: 1.2rem;
  font-family: DINAlternate-Bold, DINAlternate;
  font-weight: bold;
  color: #ffffff;
  line-height: 1.4rem;
  letter-spacing: 1px;
  text-shadow: 0px 4px 10px rgba(0, 0, 0, 0.08);
}
.topSubTitle {
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #ffffff;
  line-height: 0.825rem;
}

.topItemDataSuperBox {
  margin-top: 0.35rem;
  display: flex;
  flex-direction: row;
  width: 16.25rem;
  height: 2.5rem;
  background: #ffffff;
  border-radius: 10px;
  align-items: center;
  justify-content: space-around;
}
.itemDataBox {
  margin-top: 0.75rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 0.75rem;
  justify-content: space-between;
}
.itemDataBox_topText {
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.6rem;
}
.itemDataBox_bottomText {
  margin-top: 0.375rem;
  font-size: 20px;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 18px;
}
.vSepLine {
  width: 0.7px;
  height: 1.5rem;
  background-color: #e5e5e5;
}

.itemsBox {
  margin-top: 0.75rem;
  width: 101%;
}

.alertContantBox1 {
  /* background-color: aqua; */
  margin-top: -0.8rem;
  width: 100%;
  /* height: 1.25rem; */
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.alertTitle {
  font-size: 0.45rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.45rem;
}
.closeAlertIcon {
  width: 0.4rem;
  height: 0.4rem;
}
.alertSectionBox1 {
  margin-top: 0.75rem;
  width: 100%;
  /* height: 1.25rem; */
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  background-color: #fff8e6;
  border-radius: 0.1rem;
  height: 2.25rem;
}

.itemDataBox_topText_alert {
  margin-top: 0.25rem;
  font-size: 0.45rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.45rem;
}

.itemDataBox_bottomText_alert {
  margin-top: 0.375rem;
  font-size: 0.35rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.35rem;
}
.vSepLine_alert {
  width: 0.7px;
  height: 1.15rem;
  background-color: #e5e5e5;
}

.alertSectionBox2 {
  margin-top: 0.75rem;
  width: 100%;
  /* height: 1.25rem; */
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  border-radius: 0.1rem;
}
.switchBtnBox {
  border-style: solid;
  border-width: 0.025rem;
  border-color: #f7b500;
  width: 4rem;
  border-radius: 0.5rem;
  height: 1rem;
  display: flex;
  flex-direction: row;
  background-color: #fff8e6;
}
.switchBtn_off {
  background-color: #fff8e6;
  border-radius: 0.5rem;
  width: 50%;
  border-style: none;
  font-size: 0.4rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #f7b500;
  line-height: 0.4rem;
}
.switchBtn_on {
  background-color: #f7b500;
  border-radius: 0.5rem;
  width: 50%;
  border-style: none;
  font-size: 0.4rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #fff;
  line-height: 0.4rem;
}
.unPledgeAction {
  padding-left: 0.4rem;
  padding-right: 0.4rem;
  height: 1rem;
  border-radius: 0.5rem;
  border-style: none;
  background-color: #f7b500;
  font-size: 0.4rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #ffffff;
  line-height: 0.4rem;
}

.itemsSuperBox {
  margin-top: 0.5rem;
  border-radius: 0.25rem;
  border-style: solid;
  border-color: #ececec;
  width: 100%;
  border-width: 0.025rem;
}

.alertSectionBox3 {
  margin-top: 0.5rem;
  width: 100%;
  /* height: 1.25rem; */
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.alertSectionBox3_textBox1 {
  /* background-color: blue; */
  width: 33.33%;
}

.alertSectionBox3_textBox2 {
  /* background-color: #f7b500; */
  display: flex;
  justify-content: center;
  width: 33.33%;
}

.alertSectionBox3_textBox3 {
  /* background-color: aqua; */
  display: flex;
  flex-direction: row-reverse;
  width: 33.33%;
}
.alertSectionBox3_text1 {
  margin-left: 0.525rem;
  font-size: 0.45rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.45rem;
}
.alertSectionBox3_text2 {
  font-size: 0.45rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.45rem;
}

.alertSectionBox3_text3 {
  margin-right: 0.525rem;
  font-size: 0.45rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.45rem;
}

.selectItemSepLine {
  margin-top: 0.5rem;
  width: 100%;
  background-color: #f1f1f1;
  height: 0.025rem;
}

.alertTip1 {
  margin-top: 0.5rem;
  font-size: 0.35rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.35rem;
}
.alertTip2 {
  margin-top: 0.35rem;
  font-size: 0.35rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.35rem;
}

.bottomDesBox {
  margin-top: 1.5rem;
  width: 100%;
  /* height: 1.25rem; */
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin-bottom: 2.5rem;
}

.farmsDesImg {
  width: 4.05rem;
  height: 5.875rem;
}

.farmsDesText {
  padding-top: 0.25rem;
  padding-right: 0.5rem;
  padding-left: 0.5rem;
  padding-bottom: 0.25rem;
  /* margin-top: 0.5rem; */
  font-size: 0.45rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.7rem;
  background-color: white;
  border-radius: 0.25rem;
}

.farmsDesTextBox {
  margin-top: -1.2rem;
  align-items: center;
  /* margin-left: 0.5rem; */
  display: flex;
  flex-direction: row;
}
@media screen and (-webkit-min-device-pixel-ratio: 1) and (min-width: 1000px) {
  .back {
    display: flex;
    flex-direction: column;
    /* height: 100%; */
    width: 100%;
    background-color: #f7faff;
    /* background-color: #ea4ae0; */
  }

  .contantBack {
    /* background-color: #ea4ae0; */
    align-items: center;
    margin-top: 3.1rem;
    z-index: 100;
    display: flex;
    flex-direction: column;
    /* height: 100%; */
    width: 90%;
    margin-left: 5%;
    overflow-x: hidden;
  }

  .contantList {
    align-items: center;
    /* margin-top: 3.1rem; */
    z-index: 100;
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 90%;
    margin-left: 5%;
    overflow-x: hidden;
  }

  .topBackBox {
    position: relative;
  }

  .topImg {
    width: 100%;
    height: 8.2rem;
  }
  .topBackContant {
    top: 0%;
    left: 0%;
    height: 100%;
    position: absolute;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .topTitle {
    margin-top: 0.5rem;
    font-size: 1.5rem;
    font-family: DINAlternate-Bold, DINAlternate;
    font-weight: bold;
    color: #ffffff;
    line-height: 1.75rem;
    letter-spacing: 2px;
    text-shadow: 0px 4px 10px rgba(0, 0, 0, 0.08);
  }
  .topSubTitle {
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #ffffff;
    line-height: 0.625rem;
  }

  .topItemDataSuperBox {
    margin-top: 0.625rem;
    display: flex;
    flex-direction: row;
    width: 36rem;
    height: 3rem;
    background: #ffffff;
    border-radius: 10px;
    align-items: center;
    justify-content: space-around;
  }
  .itemDataBox {
    margin-top: 0.75rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 0.75rem;
    justify-content: space-between;
  }
  .itemDataBox_topText {
    font-size: 0.6rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
  }
  .itemDataBox_bottomText {
    margin-top: 0.525rem;
    font-size: 0.4rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
  }
  .vSepLine {
    width: 0.7px;
    height: 1.5rem;
    background-color: #e5e5e5;
  }

  .itemsBox {
    margin-top: 1.275rem;
    width: 101%;
  }

  .alertContantBox1 {
    /* background-color: aqua; */
    margin-top: -0.8rem;
    width: 100%;
    /* height: 1.25rem; */
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .alertTitle {
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.45rem;
  }
  .closeAlertIcon {
    width: 0.4rem;
    height: 0.4rem;
  }
  .alertSectionBox1 {
    margin-top: 0.75rem;
    width: 100%;
    /* height: 1.25rem; */
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    background-color: #fff8e6;
    border-radius: 0.1rem;
    height: 2.25rem;
  }

  .itemDataBox_topText_alert {
    margin-top: 0.25rem;
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.45rem;
  }

  .itemDataBox_bottomText_alert {
    margin-top: 0.375rem;
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.35rem;
  }
  .vSepLine_alert {
    width: 0.7px;
    height: 1.15rem;
    background-color: #e5e5e5;
  }

  .alertSectionBox2 {
    margin-top: 0.75rem;
    width: 100%;
    /* height: 1.25rem; */
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    border-radius: 0.1rem;
  }
  .switchBtnBox {
    border-style: solid;
    border-width: 0.025rem;
    border-color: #f7b500;
    width: 4rem;
    border-radius: 0.5rem;
    height: 1rem;
    display: flex;
    flex-direction: row;
    background-color: #fff8e6;
  }
  .switchBtn_off {
    background-color: #fff8e6;
    border-radius: 0.5rem;
    width: 50%;
    border-style: none;
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #f7b500;
    line-height: 0.4rem;
  }
  .switchBtn_on {
    background-color: #f7b500;
    border-radius: 0.5rem;
    width: 50%;
    border-style: none;
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #fff;
    line-height: 0.4rem;
  }
  .unPledgeAction {
    padding-left: 0.4rem;
    padding-right: 0.4rem;
    height: 1rem;
    border-radius: 0.5rem;
    border-style: none;
    background-color: #f7b500;
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #ffffff;
    line-height: 0.4rem;
  }

  .itemsSuperBox {
    margin-top: 0.5rem;
    border-radius: 0.25rem;
    border-style: solid;
    border-color: #ececec;
    width: 100%;
    border-width: 0.025rem;
  }

  .alertSectionBox3 {
    margin-top: 0.5rem;
    width: 100%;
    /* height: 1.25rem; */
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  .alertSectionBox3_textBox1 {
    /* background-color: blue; */
    width: 33.33%;
  }

  .alertSectionBox3_textBox2 {
    /* background-color: #f7b500; */
    display: flex;
    justify-content: center;
    width: 33.33%;
  }

  .alertSectionBox3_textBox3 {
    /* background-color: aqua; */
    display: flex;
    flex-direction: row-reverse;
    width: 33.33%;
  }
  .alertSectionBox3_text1 {
    margin-left: 0.525rem;
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.45rem;
  }
  .alertSectionBox3_text2 {
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.45rem;
  }

  .alertSectionBox3_text3 {
    margin-right: 0.525rem;
    font-size: 0.45rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.45rem;
  }

  .selectItemSepLine {
    margin-top: 0.5rem;
    width: 100%;
    background-color: #f1f1f1;
    height: 0.025rem;
  }

  .alertTip1 {
    margin-top: 0.5rem;
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.35rem;
  }
  .alertTip2 {
    margin-top: 0.35rem;
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.35rem;
  }

  .bottomDesBox {
    margin-top: 1.5rem;
    width: 100%;
    /* height: 1.25rem; */
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-bottom: 2.5rem;
  }

  .farmsDesImg {
    width: 4.05rem;
    height: 5.875rem;
  }

  .farmsDesText {
    padding-top: 0.25rem;
    padding-right: 0.5rem;
    padding-left: 0.5rem;
    padding-bottom: 0.25rem;
    /* margin-top: 0.5rem; */
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.7rem;
    background-color: white;
    border-radius: 0.25rem;
  }

  .farmsDesTextBox {
    margin-top: -1.2rem;
    align-items: center;
    /* margin-left: 0.5rem; */
    display: flex;
    flex-direction: row;
  }
}
</style>
