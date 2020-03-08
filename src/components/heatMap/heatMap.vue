<!-- 热力图 -->

<style lang="stylus" scoped>
.heatMap {
  height: 660px;
  // background url('../../assets/bg.jpg') no-repeat
  background-size: 100% 100%;
  color: white;
}

.main {
  // width 100%
  height: calc(100% - 100px);
  // margin-top -15px
}

.anchorBL {
  display: none;
}
</style>

<template lang="html">
<div class="heatMap">
  <div class="main"></div>
</div>
</template>

<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=fiiEt6XxW84BwgMfHg7xxy5cUy5tU5vx"></script>
<script>
import axios from "axios";
import echarts from "echarts";
import bmap from "echarts/extension/bmap/bmap.js";
import china from "echarts/map/js/china";
import header from "components/header/header";

export default {
  methods: {
    _init(options) {
      this.myChart = echarts.init(
        document.querySelector(".heatMap .main"),
        "dark"
      );
      this.myChart.setOption(options);
    }
  },
  mounted() {
    axios.get("static/data/datacoord.json").then(res => {
      var points = res.data.map(function(seg) {
        return seg.concat([0.2]);
      });
      let options = {
        backgroundColor: "transparent",
        animation: false,
        bmap: {
          center: [116.46, 39.92],
          // center: [120.13066322374, 30.240018034923],
          zoom: 11,
          roam: true,
          mapStyle: {
            style: "bluish"
          }
        },
        visualMap: {
          show: false,
          top: "top",
          min: 0,
          max: 5,
          seriesIndex: 0,
          calculable: true,
          inRange: {
            color: ["blue", "blue", "green", "yellow", "red"]
          }
        },
        series: [
          {
            type: "heatmap",
            coordinateSystem: "bmap",
            data: points,
            pointSize: 5,
            blurSize: 6
          }
        ]
      };
      this._init(options);
    });
  }
};
</script>