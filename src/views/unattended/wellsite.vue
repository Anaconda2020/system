<template>
  <div class="wellsite">
    <div class="wellsite_left">
      <span class="wellsite_left_title">{{ this.$route.query.name }}井场</span>
      <div class="wellsite_left_dailyData">
        <p class="wellsite_left_dailyData_p">
          日产液量:
          <span style="color: #ec8e25">{{ this.wellSite1.drLiquidProd }}</span>
          m<sup>3</sup>
        </p>
        <p class="wellsite_left_dailyData_p">
          日注水量:
          <span style="color: #ec8e25" v-if="this.wellSite2">
            {{ this.wellSite2.drWaterInjection }}
          </span>
          m<sup>3</sup>
        </p>
        <p class="wellsite_left_dailyData_p">
          电子巡检次数: <span style="color: #ec8e25">0</span> 次
        </p>
      </div>
      <div class="wellsite_left_condition">
        <i class="iconfont icon-yujing_gaoliang wellsite_left_condition_icon" />
        <span class="wellsite_left_condition_title"> 工况预警 </span>
      </div>
      <div class="wellsite_left_condition_details">
        <ul :style="{ marginTop: marginTop + 'px' }">
          <li
            v-for="(item, index) in conditionData"
            :key="index"
            class="wellsite_left_condition_details_li"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
      <div class="wellsite_left_condition">
        <i class="iconfont icon-yujing_gaoliang wellsite_left_condition_icon" />
        <span class="wellsite_left_condition_title"> 监控预警 </span>
      </div>
      <div class="wellsite_left_condition_details">
        <ul :style="{ marginTop: marginTop + 'px' }">
          <li
            v-for="(item, index) in conditionData"
            :key="index"
            class="wellsite_left_condition_details_li"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
      <div class="wellsite_left_img">
        <!-- <iframe
          :src="this.wellSiteVideo.videoLink"
          frameborder="0"
          allow="autoplay;encrypted-media"
          allowfullscreen
          style="height: 100%; width: 100%; margin-top: 1%"
        >
        </iframe> -->
        <img
          src="../../assets/images/monitor.jpg"
          style="height: 100%; width: 100%; margin-top: 1%"
        />
      </div>
      <div class="wellsite_left_video">
        <div
          class="wellsite_left_video_container"
          v-for="(item, index) in this.wellSiteNumber"
          :key="index"
        >
          <div class="wellsite_left_video_container_text">
            <i
              class="iconfont icon-shexiangtou"
              style="color: green; font-size: 28px"
            />正常
          </div>
        </div>
        <!-- <div
          class="wellsite_left_video_container"
          v-for="(item, index) in 1"
          :key="index"
        >
          <div>
            <i
              class="iconfont icon-shexiangtou"
              style="color: red; font-size: 30px"
            />异常
          </div>
        </div> -->
      </div>
    </div>
    <div class="wellsite_right">
      <div class="wellsite_right_oilWell">
        <div
          class="wellsite_right_oilWell_details"
          v-for="(item, index) in this.wellSiteOilData"
          :key="index"
        >
          <div class="wellsite_right_oilWell_details_title">
            {{ item.wellName }}采油井
          </div>
          <img
            src="../../assets/images/oilWell.gif"
            class="wellsite_right_oilWell_details_img"
            @click="gotoSingleWell(item.wellName, item.wellId)"
          />
          <div class="wellsite_right_oilWell_details_dec">
            <span class="wellsite_right_waterWell_details_dec_span"
              >当日产液:
              <span style="color: #2cab6f">{{ item.drLiquidProd }}</span> m<sup
                >3</sup
              ></span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >动液面:
              <span style="color: #2cab6f">{{ item.fluidLevel }}</span> m<sup
                >3</sup
              ></span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >工况诊断: <span style="color: #2cab6f">XXXXXX</span></span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >异常情况: <span style="color: #e62c2c"> 供液不足</span></span
            >
          </div>
        </div>
      </div>
      <div class="wellsite_right_waterWell">
        <div
          class="wellsite_right_waterWell_details"
          v-for="(item, index) in this.wellSiteWaterData"
          :key="index"
        >
          <div class="wellsite_right_waterWell_details_title">
            {{ item.wellName }}注水井
          </div>
          <img
            src="../../assets/images/waterWell.png"
            class="wellsite_right_waterWell_details_img"
            @click="gotoSingleWell(item.wellName, item.wellId)"
          />
          <div class="wellsite_right_waterWell_details_dec">
            <span class="wellsite_right_waterWell_details_dec_span"
              >瞬时注量:
              <span style="color: #2cab6f">0</span> m<sup>3</sup>/h</span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >当日注水:
              <span style="color: #2cab6f" v-if="item">
                {{ item.drWaterInjection }}
              </span>
              m<sup>3</sup></span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >累计注水: <span style="color: #2cab6f">0</span> m<sup
                >3</sup
              ></span
            >
            <span class="wellsite_right_waterWell_details_dec_span"
              >异常情况: <span style="color: #e62c2c"> 超注</span></span
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 井场汇总
      wellSite1: {},
      wellSite2: {},
      // 工况预警
      conditionData: [
        {
          name: "XXXX出现XXXX预警情况",
        },
        {
          name: "XXXX出现XXXX预警情况",
        },
        {
          name: "XXXX出现XXXX预警情况",
        },
        {
          name: "XXXX出现XXXX预警情况",
        },
        {
          name: "XXXX出现XXXX预警情况",
        },
      ],
      // 监控预警
      wellSiteVideoData: [],
      // 油井汇总
      wellSiteOilData: [],
      // 水井汇总
      wellSiteWaterData: [],
      // 视频数量
      wellSiteNumber: [],
      marginTop: 0,
      timer: "", // 定时器
    };
  },
  created() {
    // this.timer = setInterval(this.showNotice, 200); //100表示间隔时间，数字越大滚得越快
    this.wellSiteInit();
    this.wellSiteVideoInit();
    console.log(this.$route);
  },
  methods: {
    // 井场汇总信息
    wellSiteInit() {
      this.getRequest(
        "/wellSits/wellSit/WellSitList?sTime=2020-11-18&wellSitName=" +
          this.$route.query.name
      ).then((resp) => {
        if (resp) {
          // 左侧井场汇总信息
          this.wellSite1 = resp.data.wellSitInfo;
          this.wellSite2 = resp.data.waterSitInfo;
          // 右侧详细信息
          this.wellSiteOilData = resp.data.wellSitInfoList;
          this.wellSiteWaterData = resp.data.waterSitInfoList;
        }
      });
    },
    // 视频信息
    wellSiteVideoInit() {
      this.getRequest(
        "/unattended/monitoring/getVideoLink?wellSitName=" +
          this.$route.query.name
      ).then((resp) => {
        if (resp) {
          this.wellSiteNumber = resp.data;
        }
      });
    },
    showNotice() {
      this.marginTop -= 1;
      if (this.marginTop < -20) {
        // 滚上去36px后把前两条数据拉下来
        this.conditionData.push(this.conditionData[0]);
        this.conditionData.shift();
        this.marginTop = 0;
      }
    },
    // 返回采油站页面
    gotoOilStation() {
      this.$router.replace("/unattended/oilStation");
    },
    // 跳转到单井页面
    gotoSingleWell(val1, val2) {
      this.$router.push({
        path: "/unattended/singleWell",
        query: {
          name: val1,
          id: val2,
        },
      });
    },
  },
};
</script>

<style lang="less" scoped>
@import "../../assets/css/unattended/wellsite.css";
</style>
<style>
.wellsite_left_condition_details ul li:before {
  display: inline-block;
  content: "●";
  padding-right: 10px;
  color: rgb(202, 137, 137);
  font-size: 20px;
}
</style>
