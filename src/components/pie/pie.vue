<!-- 饼图 -->
<style lang="stylus" scoped>
.pie {
  height: 100vh;
  // background url('../../assets/bg.jpg') no-repeat
  background-size: 100% 100%;
  color: white;
}

.main {
  // width 100%
  height: calc(100% - 100px);
  // margin-top -15px
}
</style>


<template>
  <div class="pie">
    <div class="main"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import header from "components/header/header";
import filter from "components/filter/filter";

export default {
  data() {
    return {
      legendArr: [],
      color: this.$store.state.color,
      styleArr: [],
      myChart: {},
      name: "饼图"
    };
  },
  methods: {
    _init() {
      this.legendArr = this.myChart.getOption().series;
      this.legendArr.forEach(data => {
        data.selected = true;
      });
      this.$root.charts.push(this.myChart);
      window.addEventListener(
        "resize",
        function() {
          this.myChart.resize();
        }.bind(this)
      );
    }
  },
  mounted() {
    // 基于准备好的dom，初始化echarts实例
    this.myChart = echarts.init(document.querySelector(".pie .main"));
    this.myChart.setOption({
      // title: {
      //   text: "短信分类比例饼图",
      //   x: "center",
      //   align: "right",
      //   textStyle: {
      //     color: "#fff"
      //   }
      // },
      tooltip: {
        trigger: "item",
        formatter: "{a} <br/>{b}: {c} ({d}%)"
      },
      legend: {
        orient: "vertical",
        x: "left",
        textStyle: {
          color: "#ff"
        },
        data: [
          "广告推销",
          "诈骗短信",
          "非法服务",
          "色情服务",
          "其它",
          "房产广告",
          "商品销售",
          "电信诈骗",
          "赌博诈骗",
          "代开发票",
          "非法贷款",
          "办假证"
        ]
      },
      series: [
        {
          name: "短信分类",
          type: "pie",
          selectedMode: "single",
          radius: [0, "30%"],

          label: {
            normal: {
              show: false
            }
          },
          // labelLine: {
          //   normal: {
          //     show: false
          //   }
          // },
          data: [
            { value: 217574, name: "广告推销", selected: true },
            { value: 721789, name: "诈骗短信" },
            { value: 1890700, name: "非法服务" },
            { value: 309684, name: "色情服务" },
            { value: 119669, name: "其它" }
          ]
        },
        {
          name: "短信分类",
          type: "pie",
          radius: ["40%", "55%"],
          data: [
            { value: 131974, name: "房产广告" },
            { value: 2016, name: "商品销售" },
            { value: 83584, name: "服务推广" },
            { value: 720822, name: "电信诈骗" },
            { value: 371, name: "赌博诈骗" },
            { value: 596, name: "招聘诈骗" },
            { value: 1882010, name: "代开发票" },
            { value: 8665, name: "非法贷款" },
            { value: 25, name: "办假证" },
            { value: 309684, name: "色情服务" },
            { value: 119669, name: "其它" }
          ]
        }
      ]
    });
    this._init();
  }
};
</script>
