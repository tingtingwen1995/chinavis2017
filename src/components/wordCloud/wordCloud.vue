<!--词云图 -->
<style lang="stylus" scoped>
.wordCloud {
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
  <div class="wordCloud">
    <div class="main"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import axios from "axios";
import wordcloud from "echarts-wordcloud";
import header from "components/header/header";
import filter from "components/filter/filter";

export default {
  data() {
    return {
      legendArr: [],
      color: this.$store.state.color,
      myChart: {},
      name: "词云图"
    };
  },
  methods: {
    _init() {
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
    this.myChart = echarts.init(document.querySelector(".wordCloud .main"));
    axios.get("static/data/keyword.json").then(res => {
      const { data = {} } = res;
      this.myChart.setOption({
        title: {
          text: "短信内容(左)和短信分类比例图(右)",
          x: "center",
          align: "right",
          textStyle: {
            color: "#fff"
          },
          subtext: "数据来自QHNet公司推出的手机卫士应用软件积累的垃圾短信样本"
        },
        series: [
          {
            type: "wordCloud",
            gridSize: 2,
            sizeRange: [12, 50],
            rotationRange: [-90, 90],
            shape: "pentagon",
            // shape: "circle",
            textStyle: {
              normal: {
                color: function() {
                  return (
                    "rgb(" +
                    [
                      Math.round(Math.random() * 255),
                      Math.round(Math.random() * 255),
                      Math.round(Math.random() * 255)
                    ].join(",") +
                    ")"
                  );
                }
              },
              emphasis: {
                shadowBlur: 10,
                shadowColor: "#fff"
              }
            },
            data: data
          }
        ]
      });
    })
    this._init();
  }
};
</script>
