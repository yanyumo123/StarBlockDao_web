<template>
  <el-row :gutter="gutterSpace" class="row-bg">
    <el-col v-for="(item, index) in items" :span="rowNum">
      <div class="grid-content bg-purple">
        <div class="contantDetailBox">
          <!-- top -->
          <div class="contantDetailTopBox">
            <div class="contantDetailTopBox_leftBox">
              <p class="contantDetailTopBox_leftBox_title">赚取：</p>
              <img class="contantDetailTopBox_leftBox_img" src="@/assets/img/farms/linkIcon1.png" />
              <p class="contantDetailTopBox_leftBox_imgDes">STB</p>
              <p class="contantDetailTopBox_leftBox_des">10,000/月</p>
            </div>
            <div class="contantDetailTopBox_rightBox">
              <p class="contantDetailTopBox_rightBox_text">346,523 区块后结束</p>
            </div>
          </div>
        </div>

        <!-- 奖励 -->
        <div class="contantDetailSection1">
          <div class="contantDetailSection1_leftBox">
            <img class="contantDetailSection1_leftBox_img" src="@/assets/img/farms/collectionIcon.png"/>
            <div class="contantDetailSection1_leftBox_subBox">
              <p class="contantDetailSection1_leftBox_subBox_topText">BakerLion</p>
              <div class="linkIconBox">
                <img class="linkIcon" src="@/assets/img/farms/linkIcon1.png" />
                <img class="linkIcon1" src="@/assets/img/farms/linkIcon2.png" />
                <img class="linkIcon1" src="@/assets/img/farms/linkIcon3.png" />
                <img class="linkIcon1" src="@/assets/img/farms/linkIcon4.png" />
              </div>
            </div>
          </div>
          <div class="contantDetailSection1_rightBox">
            <p class="contantDetailSection1_rightBox_topText">10.2 STB</p>
            <p class="contantDetailSection1_rightBox_bottomText">单NFT奖励/月</p>
          </div>
        </div>

        <!-- 抵押总量、TVL -->
        <div class="contantDetailSection2">
          <p class="contantDetailSection2_leftText">
            抵押总量
            <span class="contantDetailSection2_leftText1">2,235</span>
          </p>
          <p class="contantDetailSection2_rightText">
            TVL
            <span class="contantDetailSection2_rightText1">1245 ETH</span>
          </p>
        </div>
        <!-- 抵押、解抵押 -->
        <div class="pledgeBtnBox">
          <button class="pledgeBtn" @click="pledgeBtnAction(item)">抵押(9)</button>
          <button class="unPledgeBtn" @click="pledgeBtnAction(item)">解抵押(10)</button>
        </div>
        <!-- 领取奖励 -->
        <button class="getAwardBtn">领取奖励</button>
        <button class="getBonuBtn">领取分红</button>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import qs from "qs";

import {
  getLocalStorage,
  setLocalStorage,
  isLogin,
  localAccount,
  localUserLoginObject,
  getETHPriceItemCell,
  checkChainIdError
} from "@/common/utils";
export default {
  name: "Farmitem",
  components: {},
  data() {
    var rowNum = 0;
    if (document.documentElement.clientWidth <= 700) {
      rowNum = 24;
    }
    // else if (
    //   document.documentElement.clientWidth > 700 &&
    //   document.documentElement.clientWidth < 1920
    // ) {
    //   rowNum = 8;
    // }
    else {
      rowNum = 6;
    }
    return {
      windowWidth: document.documentElement.clientWidth, //实时屏幕宽度
      rowNum: rowNum,
      gutterSpace: document.documentElement.clientWidth > 600 ? 27 : 10,
      isSave: false,
      day: "",
      hour: "",
      minute: "",
      second: "",
      stopCountDown: false,
      isStartCountDown: false,
      isShowCountDown: false,
      isShowEndSell: false,
      serviceRate: 0,
      isRequestServiceRate: false
    };
  },
  created() {
    // alert(document.documentElement.clientWidth);
    // if (this.serviceRate == 0) {
    // this.requestServiceRate();
    // }
  },
  computed: {
    getDayStr() {
      this.getTimeOfType(1);
      return this.day;
    },
    getHourStr() {
      this.getTimeOfType(2);
      return this.hour;
    },
    getMinuteStr() {
      this.getTimeOfType(3);
      return this.minute;
    },
    getSecondStr() {
      this.getTimeOfType(4);
      return this.second;
    }
  },
  props: {
    items: {
      type: Array,
      default() {
        return [];
      }
    },
    isRecommend: {
      type: Boolean,
      default() {
        return false;
      }
    },
    itemsHeight: {
      type: Boolean,
      default: true
    }
  },
  watch: {
    windowWidth(val) {
      // 为了避免频繁触发resize函数导致页面卡顿，使用定时器
      // if (!this.timer) {
      // 一旦监听到的screenWidth值改变，就将其重新赋给data里的screenWidth
      this.windowWidth = val;
      this.timer = true;
      let that = this;
      // setTimeout(function () {
      // 打印screenWidth变化的值
      // console.log("实时屏幕宽度：", val, that.screenWidth);
      // that.rowNum = that.windowWidth > 600 ? 6 : 24;

      var rowNum = 0;
      if (that.windowWidth <= 700) {
        rowNum = 24;
      }
      // else if (that.windowWidth > 700 && that.windowWidth < 1440) {
      //   rowNum = 12;
      // } else if (that.windowWidth >= 1440 && that.windowWidth < 1920) {
      //   rowNum = 8;
      // }
      else {
        rowNum = 6;
      }
      that.rowNum = rowNum;
      that.gutterSpace = that.windowWidth > 600 ? 27 : 6;

      // var html = document.documentElement; //获取到html元素
      // var hWidth = html.getBoundingClientRect().width; //获取到html的宽度
      // if (hWidth < 1000) {
      //   //   hWidth = 1000;
      //   //   html.style.fontSize = hWidth / 7.5 + "px";
      //   // html.style.fontSize = hWidth / 75 + "px";
      //   html.style.fontSize = 40 + "px";
      // }
      // if (hWidth >= 1000) {
      //   //   hWidth = 2000;
      //   //   html.style.fontSize = hWidth / 15 + "px";
      //   html.style.fontSize = 40 + "px";
      // }
    }
  },

  mounted() {
    var that = this;
    // <!--把window.onresize事件挂在到mounted函数上-->
    window.onresize = () => {
      return (() => {
        window.fullHeight = document.documentElement.clientHeight;
        window.fullWidth = document.documentElement.clientWidth;
        that.windowHeight = window.fullHeight; // 高
        that.windowWidth = window.fullWidth; // 宽
      })();
    };
  },
  methods: {
    pledgeBtnAction(item) {
      this.$bus.$emit("alertAction", "1");
    },
    imgLoad() {
      // 发射事件总线
      this.$bus.$emit("imgLoad");
    },
    // 跳转到详情页
    goodItemClick(item) {
      this.$emit("itemClick", item);
      var id = "";
      if (item.uuid) {
        id = item.uuid;
      }
      if (item.upChain) {
        if (item.contractAddress) {
          id = item.contractAddress;
        }
      }

      // textNetwork_Name();
      this.$router.push({ name: "collection", params: { id: id } });
    }
  }
};
</script>

<style scoped>
.el-col {
  margin-bottom: 0.75rem;
  border-radius: 4px;
  /* background-color: antiquewhite; */
}
.bg-purple-dark {
  background: #99a9bf;
  border-color: lightgray;
  border-width: 0.05px;
}
.bg-purple {
  background: white;
  border-color: #99a9bf;
  /* border-width: 0.05px; */
  /* border-style: solid; */
  box-shadow: 0px 5px 12px 0px rgb(207 214 230 / 30%);
  margin-left: 2px;
  margin-right: 2px;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  cursor: pointer;
  border-radius: 10px;
  /* min-height: 80px; */
  display: flex;
  /* flex-direction: column; */
  flex-wrap: wrap;
  z-index: -1;
}
.row-bg {
  padding: 0 0.25rem;
  /* background-color: white; */
  /* background-color: tomato; */
}

.contantDetailBox {
  width: 100%;
  /* background-color: aquamarine; */
  display: flex;
  flex-direction: column;
  align-items: center;
  /* margin-left: 0.5rem; */
  /* margin-right: 0.5rem; */
  height: 100%;
}
.contantDetailTopBox {
  /* background-color: aqua; */
  margin-top: 0.5rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
.contantDetailTopBox_leftBox {
  margin-left: 0.5rem;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.contantDetailTopBox_leftBox_title {
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #111111;
  line-height: 0.825rem;
}
.contantDetailTopBox_leftBox_img {
  width: 0.75rem;
  height: 0.75rem;
}
.contantDetailTopBox_leftBox_imgDes {
  margin-left: 0.15rem;
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.825rem;
}
.contantDetailTopBox_leftBox_des {
  margin-left: 0.1rem;
  font-size: 0.5rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.7rem;
}
.contantDetailTopBox_rightBox {
  margin-right: 0.5rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  background-color: #fff8e6;
  border-radius: 4px;
}
.contantDetailTopBox_rightBox_text {
  margin-left: 0.25rem;
  margin-right: 0.25rem;
  font-size: 0.5rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #f7b500;
  line-height: 0.7rem;
}
.contantDetailSection1 {
  margin-top: 0.425rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: #fff8e6;
  border-radius: 0.25rem;
  justify-content: space-between;
  height: 3.25rem;
  margin-left: 2.5%;
  width: 95%;
}

.contantDetailSection1_leftBox {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.contantDetailSection1_leftBox_img {
  margin-left: 0.25rem;
  width: 2.25rem;
  height: 2.25rem;
}
.contantDetailSection1_leftBox_subBox {
  margin-left: 0.5rem;
  display: flex;
  flex-direction: column;
  height: 2.25rem;
  justify-content: space-between;
}
.contantDetailSection1_leftBox_subBox_topText {
  font-size: 0.7rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 1rem;
}

.linkIconBox {
  display: flex;
  flex-direction: row;
}

.linkIcon {
  width: 1rem;
  height: 1rem;
}

.linkIcon1 {
  margin-left: 0.25rem;
  width: 1rem;
  height: 1rem;
}

.contantDetailSection1_rightBox {
  height: 2.25rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-right: 0.25rem;
  justify-content: space-between;
}
.contantDetailSection1_rightBox_topText {
  width: 100%;
  text-align: right;
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #111111;
  line-height: 0.825rem;
}
.contantDetailSection1_rightBox_bottomText {
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #f7b500;
  line-height: 0.825rem;
}

.contantDetailSection2 {
  margin-top: 0.5rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: #fff8e6;
  border-radius: 0.25rem;
  justify-content: space-between;
  height: 2rem;
  margin-left: 2.5%;
  width: 95%;
}
.contantDetailSection2_leftText {
  margin-left: 0.5rem;
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.825rem;
}
.contantDetailSection2_leftText1 {
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #111;
  line-height: 0.825rem;
}

.contantDetailSection2_rightText {
  margin-right: 0.5rem;
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #8c9399;
  line-height: 0.825rem;
}
.contantDetailSection2_rightText1 {
  font-size: 0.6rem;
  font-family: PingFangSC-Regular, PingFang SC;
  font-weight: 400;
  color: #111;
  line-height: 0.825rem;
}
.pledgeBtnBox {
  margin-top: 0.75rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  border-radius: 0.1rem;
  justify-content: space-between;
  height: 1rem;
  margin-left: 2.5%;
  width: 95%;
}

.pledgeBtn {
  border-style: none;
  width: 45%;
  height: 1.75rem;
  background-color: #f7b500;
  border-radius: .875rem;
  color: #fff;
  font-size: .6rem;
  font-family: PingFangSC-Medium, PingFang SC;
}
.unPledgeBtn {
  border-style: solid;
  border-width: 0.025rem;
  border-color: #f7b500;
  /* width: 3.55rem; */
  width: 45%;
  height: 1.75rem;
  background-color: #fff;
  border-radius: 0.875rem;
  color: #f7b500;
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
}
.getAwardBtn {
  border-style: none;
  margin-top: 0.75rem;
  height: 1rem;
  margin-left: 2.5%;
  width: 95%;
  height: 1.75rem;
  background-color: #f7b500;
  border-radius: 0.875rem;
  color: #fff;
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
}
.getBonuBtn {
  border-style: none;
  margin-top: 0.5rem;
  height: 1.75rem;
  margin-left: 2.5%;
  width: 95%;
  height: 1rem;
  background-color: #f7b500;
  border-radius: 0.875rem;
  color: #fff;
  font-size: 0.6rem;
  font-family: PingFangSC-Medium, PingFang SC;
  margin-bottom: 0.75rem;
}
@media screen and (-webkit-min-device-pixel-ratio: 1) and (min-width: 1000px) {
  .el-col {
    /* margin-left: -0.25rem; */
    margin-bottom: 1.2rem;
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
    border-color: rgb(48, 29, 29);
    border-width: 0.05px;
  }
  .row-bg {
    /* padding: 0 -0.25rem; */
    /* background-color: white; */
    /* background-color: tomato; */
  }

  .contantDetailBox {
    width: 100%;
    /* background-color: aquamarine; */
    display: flex;
    flex-direction: column;
    align-items: center;
    /* margin-left: 0.5rem; */
    /* margin-right: 0.5rem; */
    height: 100%;
  }
  .contantDetailTopBox {
    /* background-color: aqua; */
    margin-top: 0.5rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    width: 100%;
  }
  .contantDetailTopBox_leftBox {
    margin-left: 0.5rem;
    display: flex;
    flex-direction: row;
    align-items: center;
  }
  .contantDetailTopBox_leftBox_title {
    font-size: 0.4rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #111111;
    line-height: 0.55rem;
  }
  .contantDetailTopBox_leftBox_img {
    width: 0.5rem;
    height: 0.5rem;
  }
  .contantDetailTopBox_leftBox_imgDes {
    margin-left: 0.15rem;
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.55rem;
  }
  .contantDetailTopBox_leftBox_des {
    margin-left: 0.1rem;
    font-size: 0.3rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.425rem;
  }
  .contantDetailTopBox_rightBox {
    margin-right: 0.5rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    background-color: #fff8e6;
    border-radius: 4px;
  }
  .contantDetailTopBox_rightBox_text {
    margin-left: 0.25rem;
    margin-right: 0.25rem;
    font-size: 0.3rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #f7b500;
    line-height: 0.425rem;
  }
  .contantDetailSection1 {
    margin-top: 0.425rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    background-color: #fff8e6;
    border-radius: 0.1rem;
    justify-content: space-between;
    height: 1.75rem;
    margin-left: 5%;
    width: 90%;
  }

  .contantDetailSection1_leftBox {
    display: flex;
    flex-direction: row;
    align-items: center;
  }
  .contantDetailSection1_leftBox_img {
    margin-left: 0.25rem;
    width: 1.25rem;
    height: 1.25rem;
  }
  .contantDetailSection1_leftBox_subBox {
    margin-left: 0.25rem;
    display: flex;
    flex-direction: column;
    height: 1.25rem;
    justify-content: space-between;
  }
  .contantDetailSection1_leftBox_subBox_topText {
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.4rem;
  }

  .linkIconBox {
    display: flex;
    flex-direction: row;
  }

  .linkIcon {
    width: 0.5rem;
    height: 0.5rem;
  }

  .linkIcon1 {
    margin-left: 0.25rem;
    width: 0.5rem;
    height: 0.5rem;
  }

  .contantDetailSection1_rightBox {
    height: 1.25rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-right: 0.25rem;
    justify-content: space-between;
  }
  .contantDetailSection1_rightBox_topText {
    width: 100%;
    text-align: right;
    font-size: 0.4rem;
    font-family: PingFangSC-Medium, PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.4rem;
  }
  .contantDetailSection1_rightBox_bottomText {
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #f7b500;
    line-height: 0.35rem;
  }

  .contantDetailSection2 {
    margin-top: 0.25rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    background-color: #fff8e6;
    border-radius: 0.1rem;
    justify-content: space-between;
    height: 1rem;
    margin-left: 5%;
    width: 90%;
  }
  .contantDetailSection2_leftText {
    margin-left: 0.25rem;
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.5rem;
  }
  .contantDetailSection2_leftText1 {
    font-size: 0.35rem;
    font-family: PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.5rem;
  }

  .contantDetailSection2_rightText {
    margin-right: 0.25rem;
    font-size: 0.35rem;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #8c9399;
    line-height: 0.5rem;
  }
  .contantDetailSection2_rightText1 {
    font-size: 0.35rem;
    font-family: PingFang SC;
    font-weight: 500;
    color: #111111;
    line-height: 0.5rem;
  }
  .pledgeBtnBox {
    margin-top: 0.5rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    border-radius: 0.1rem;
    justify-content: space-between;
    height: 1rem;
    margin-left: 5%;
    width: 90%;
  }

  .pledgeBtn {
    border-style: none;
    width: 45%;
    height: 1rem;
    background-color: #f7b500;
    border-radius: 0.5rem;
    color: #fff;
    font-size: 0.35rem;
    font-family: PingFangSC-Medium, PingFang SC;
  }
  .unPledgeBtn {
    border-style: solid;
    border-width: 0.025rem;
    border-color: #f7b500;
    /* width: 3.55rem; */
    width: 45%;
    height: 1rem;
    background-color: #fff;
    border-radius: 0.5rem;
    color: #f7b500;
    font-size: 0.35rem;
    font-family: PingFangSC-Medium, PingFang SC;
  }
  .getAwardBtn {
    border-style: none;
    margin-top: 0.25rem;
    height: 1rem;
    margin-left: 5%;
    width: 90%;
    height: 1rem;
    background-color: #f7b500;
    border-radius: 0.5rem;
    color: #fff;
    font-size: 0.35rem;
    font-family: PingFangSC-Medium, PingFang SC;
  }
  .getBonuBtn {
    border-style: none;
    margin-top: 0.25rem;
    height: 1rem;
    margin-left: 5%;
    width: 90%;
    height: 1rem;
    background-color: #f7b500;
    border-radius: 0.5rem;
    color: #fff;
    font-size: 0.35rem;
    font-family: PingFangSC-Medium, PingFang SC;
    margin-bottom: 0.5rem;
  }
}
</style>