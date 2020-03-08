<!-- 时间空间短信分类地区分布 -->
<style lang="stylus" scoped>
.mixLine {
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
  <div class="mixLine">
    <div class="main"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import axios from "axios";
import header from "components/header/header";
import filter from "components/filter/filter";

export default {
  data() {
    return {
      legendArr: [],
      color: this.$store.state.color,
      myChart: {},
      name: "短信类型的时空分布图"
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
    var dataMap = {};

    function dataFormatter(
      advertising,
      scamSms,
      illegalService,
      pornographicService,
      other
    ) {
      const temp = [];
      let dataA = 0;
      let dataSms = 0;
      let dataI = 0;
      let dataP = 0;
      let otherD = 0;
      advertising.map((dt, index) => {
        dt.map(datadt => {
          dataA += datadt.value;
        });
        scamSms[index].map(datadt => {
          dataSms += datadt.value;
        });
        illegalService[index].map(datadt => {
          dataI += datadt.value;
        });
        pornographicService[index].map(datadt => {
          dataP += datadt.value;
        });
        other[index].map(datadt => {
          otherD += datadt.value;
        });
        temp.push({
          title: { text: `${dt && dt[0].date}短信类型的时空分布图` },
          series: [
            { data: dt },
            { data: scamSms[index] },
            { data: illegalService[index] },
            { data: pornographicService[index] },
            { data: other[index] },
            {
              data: [
                {
                  name: "广告推销",
                  value: dataA,
                  itemStyle: { color: "#37A2DA" }
                },
                {
                  name: "诈骗短信",
                  value: dataSms,
                  itemStyle: { color: "#32C2E6" }
                },
                {
                  name: "非法服务",
                  value: dataI,
                  itemStyle: { color: "#9ADFB3" }
                },
                {
                  name: "色情服务",
                  value: dataP,
                  itemStyle: { color: "#FAD65B" }
                },
                { name: "其它", value: otherD, itemStyle: { color: "#FE9F7F" } }
              ]
            }
          ]
        });
      });
      return temp;
    }
    this.myChart = echarts.init(
      document.querySelector(".mixLine .main"),
      "light"
    );
    // this.myChart = this.myChart.init(document.querySelector(".mixLine .main"));
    axios.get("static/data/mixLine.json").then(res => {
      const { data = {} } = res;
      const {
        advertising,
        scamSms,
        illegalService,
        pornographicService,
        other
      } = data;
      const options = dataFormatter(
        advertising,
        scamSms,
        illegalService,
        pornographicService,
        other
      );
      this.myChart.setOption({
        // title: {
        //   text: "基于短信内容的时空分布图",
        //   x: "center",
        //   align: "right",
        //   textStyle: {
        //     color: "#fff"
        //   }
        // },
        baseOption: {
          textStyle: {
            color: "#fff"
          },
          lineStyle: {
            color: "#fff"
          },
          timeline: {
            // y: 0,
            itemStyle: {
              color: "#fff"
            },
            left: 5,
            right: 5,
            symbolSize: 8,
            axisType: "category",
            // realtime: false,
            // loop: false,
            autoPlay: false,
            // currentIndex: 2,
            playInterval: 1000,
            // controlStyle: {
            //     position: 'left'
            // },
            // axisType: "axisType",
            // data: [
            //   "2002-01-01",
            //   "2003-01-01",
            //   "2004-01-01",
            //   "2005-01-01",
            //   // {
            //   //   value: "2005-01-01",
            //   //   tooltip: {
            //   //     formatter: "{b} GDP达到一个高度"
            //   //   },
            //   //   symbol: "diamond",
            //   //   symbolSize: 16
            //   // },
            //   "2006-01-01",
            //   "2007-01-01",
            //   "2008-01-01",
            //   "2009-01-01",
            //   "2010-01-01"
            // ],
            data: [
              "02-23",
              "02-24",
              "02-25",
              "02-26",
              "02-27",
              "02-28",
              "03-01",
              "03-02",
              "03-03",
              "03-04",
              "03-05",
              "03-06",
              "03-07",
              "03-08",
              "03-09",
              "03-10",
              "03-11",
              "03-12",
              "03-13",
              "03-14",
              "03-15",
              "03-16",
              "03-17",
              "03-18",
              "03-19",
              "03-20",
              "03-21",
              "03-22",
              "03-23",
              "03-24",
              "03-25",
              "03-26",
              "03-27",
              "03-28",
              "03-29",
              "03-30",
              "03-31",
              "04-01",
              "04-02",
              "04-03",
              "04-04",
              "04-05",
              "04-06",
              "04-07",
              "04-08",
              "04-09",
              "04-10",
              "04-11",
              "04-12",
              "04-13",
              "04-14",
              "04-15",
              "04-16",
              "04-17",
              "04-18",
              "04-19",
              "04-20",
              "04-21",
              "04-22",
              "04-23",
              "04-24",
              "04-25",
              "04-26"
            ],
            label: {
              // formatter: function(s) {
              //   return new Date(s).getFullYear();
              // },
              color: "#fff"
            },
            lineStyle: {
              color: "fff"
            },
            controlStyle: {
              color: "fff",
              borderColor: "fff"
            }
          },
          title: {
            textStyle: {
              color: "#fff"
            },
            subtext: "数据来自QHNet公司推出的手机卫士应用软件积累的垃圾短信样本"
          },
          tooltip: {},
          legend: {
            x: "right",
            textStyle: {
              color: "#fff"
            },
            data: ["广告推销", "诈骗短信", "非法服务", "色情服务", "其它"]
            // selected: {
            //   GDP: false,
            //   金融: false,
            //   房地产: false
            // }
          },
          calculable: true,
          grid: {
            top: 80,
            bottom: 100,
            tooltip: {
              trigger: "axis",
              axisPointer: {
                type: "shadow",
                label: {
                  show: true,
                  formatter: function(params) {
                    return params.value.replace("\n", "");
                  }
                }
              }
            }
          },
          xAxis: [
            {
              type: "category",
              axisLabel: { interval: 0 },
              data: [
                "东城区",
                "西城区",
                "朝阳区",
                "丰台区",
                "石景山区",
                "海淀区",
                "门头沟区",
                "房山区",
                "通州区",
                "顺义区",
                "昌平区",
                "大兴区",
                "怀柔区",
                "平谷区",
                "密云区",
                "延庆区"
              ],
              splitLine: { show: false }
            }
          ],
          yAxis: [
            {
              type: "value",
              name: "短信数量"
            }
          ],
          series: [
            { name: "广告推销", type: "bar" },
            { name: "诈骗短信", type: "bar" },
            { name: "非法服务", type: "bar" },
            { name: "色情服务", type: "bar" },
            { name: "其它", type: "bar" },
            {
              name: "GDP占比",
              type: "pie",
              center: ["75%", "35%"],
              radius: "28%",
              z: 100
            }
          ]
        },
        options: options
      });
    });
    this._init();
  }
};
</script>
