<template lang="html">
  <div class="dashboard">
    <div class="flex-container column">
        <div class="item one" @click="clickChart('1')" style="transform: translate(-31%,-33.5%) scale(0.33)">
          <heatMap></heatMap>
        </div>
        <div class="item two" @click="clickChart('2')" style="transform: translate(-31%,0.5%) scale(0.33)">
          <div style="height: 100%; width: 45%;"><wordCloud></wordCloud></div>
          <div style="margin-left: 40%; transform: translate(20%, -90%); height: 100%;width: 50%;">
            <pie></pie>
          </div>
        </div>
        <div class="item three" @click="clickChart('3')" style="transform: translate(-31%,34.5%) scale(0.33)">
          <mixLine></mixLine>
        </div>
        <div class="item four active" @click="clickChart('4')" style="transform: translate(35%, 0) scale(1)">
          <funnel></funnel>
        </div>
    </div>
  </div>
</template>

<script>
import column from 'components/column/column'
import line from 'components/line/line'
import multipleColumn from 'components/multipleColumn/multipleColumn'
import point from 'components/point/point'
import heat from 'components/heat/heat'
import funnel from 'components/funnel/funnel'
import heatMap from 'components/heatMap/heatMap'
import pie from 'components/pie/pie'
import singleAxis from 'components/singleAxis/singleAxis'
import wordCloud from 'components/wordCloud/wordCloud'
import mixLine from 'components/mixLine/mixLine'

export default {
  data() {
    return {
      items: []
    }
  },
  mounted() {
    this._init()
  },
  methods: {
    _resize() {
      this.$root.charts.forEach((myChart) => {
        myChart.resize()
      })
    },
    _init() {
      this.items = document.querySelectorAll('.flex-container .item')
      for (let i = 0; i < this.items.length; i++) {
        this.items[i].dataset.order = i + 1;
      }
    },
    clickChart(clickIndex) {
      let activeItem = document.querySelector('.flex-container .active')
      let activeIndex = activeItem.dataset.order
      let clickItem = this.items[clickIndex - 1]
      if (activeIndex === clickIndex) {
        return
      }
      activeItem.classList.remove('active')
      clickItem.classList.add('active')
      this._setStyle(clickItem, activeItem)
    },
    _setStyle(el1, el2) {
      let transform1 = el1.style.transform
      let transform2 = el2.style.transform
      el1.style.transform = transform2
      el2.style.transform = transform1
    }
  },
  components: {
    column,
    multipleColumn,
    point,
    'v-line': line,
    heat,
    funnel,
    heatMap,
    pie,
    singleAxis,
    wordCloud,
    mixLine
  }
}

</script>

<style lang="stylus" scoped>
*
  box-sizing: border-box;
.point,.multipleColumn,.columnChart,.line
  height 100%!important
  width 100%!important
  background none!important
// .title
//     color: #fafafa;
//     letter-spacing: 0;
//     text-shadow: 0px 1px 0px #999, 0px 2px 0px #888, 0px 3px 0px #777, 0px 4px 0px #66;
//     text-align: center
.item
    padding: 0px;
    margin: 0px;
    width: 70%;
    height: 100%;
    position absolute
    transform scale(0.33)
    text-align: center;
    transition:all 0.8s;
    background rgba(32, 32, 35, 0.5)
.dashboard
    position relative
    width 100%
    height 100%
    margin:0px;
    padding:0px;
    padding-top 2%
    background url('../../assets/bg.jpg');
    background-size 100% 100%
.flex-container.column
    position relative
    height: 98%;
    width: 100%;
    overflow: hidden;
    margin:  0 auto 100px auto;
    box-sizing: content-box;
.active
    height 100%
    width: 72%;
    margin-left: 10px;
    line-height: 300px;
// .half
//     display inline-block;
//     width: 50%;
</style>
