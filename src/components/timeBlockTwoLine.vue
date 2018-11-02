<template>
  <div class="hello w100">
    <div class="con w100">
      <div class="lineTitle displayflex">
        <div class="line blueFontColor" v-for="(i, index) in timePointFun1(msgFather)" :key="index"
             :class="{fontColorCCC: i.type === 2}">
          {{i.time | formatTime1}}
        </div>
      </div>
      <div class="linePoint" id="linePoint">
        <div class="points">
          <div class="point" v-for="(item, index) in timePointFun1(msgFather)" :key="index">
            <!--<div class="circular" :class="{bgRed: item.type === 1, bgBlack: item.type === 2}"></div>-->
            <div class="circular">
              <img src="../assets/images/freeTime.png" alt="" v-if="item.type !== 2 && item.type !== 1">
              <img src="../assets/images/overTime.png" alt="" v-if="item.type === 2">
              <img src="../assets/images/yu.png" alt="" v-if="item.type === 1">
            </div>
          </div>
        </div>
        <div class="timePoint"  v-for="(line1, index) in timeLineFun1(msgFather)" :key="index" :style="{
        width: line1.width, left: line1.left}" :class="{borderBlcak: line1.border}"></div>
       <!-- <div class="timePoint" v-for="(i, index) in timeLine1"
        :style="{width: i.width, left: i.left}"></div>-->
      </div>
    </div>
    <div class="con w100">
      <div class="lineTitle displayflex">
        <div class="line blueFontColor" v-for="(i, index) in timePointFun2(msgFather)" :key="index"
             :class="{fontColorCCC: i.type === 2}">
          {{i.time |formatTime1}}
        </div>
      </div>
      <div class="linePoint" id="linePoint1">
        <div class="points">
          <div class="point" v-for="(item, index) in timePointFun2(msgFather)" :key="index">
            <div class="circular">
              <img src="../assets/images/freeTime.png" alt="" v-if="item.type !== 2 && item.type !== 1">
              <img src="../assets/images/overTime.png" alt="" v-if="item.type === 2">
              <img src="../assets/images/yu.png" alt="" v-if="item.type === 1">
            </div>
          </div>
        </div>
        <div class="timePoint" v-for="(line2, index) in timeLineFun2(msgFather)" :key="index" :style="{
        width: line2.width, left: line2.left}" :class="{borderBlcak: line2.border}"></div>
      </div>
    </div>
  </div>
</template>
<style>
  .w100{width: 100%;}
  .timePoint{
    position: absolute;
    z-index: 99;
    top: 0;
    border-bottom: 1px solid #feb166;
  }
  .borderBlcak {
    border-bottom: 1px solid #d5d5d5;
    z-index: 99;
  }
  .fontColorCCC {
    color: #808080;
  }
</style>
<script>
import {FormatDate} from '../assets/index'

export default {
  name: 'TimeBlock',
  props: {
    msgFather: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      timeLine1: [],
      timeLine2: []
    }
  },
  filters: {
    formatTime1 (time) {
      return FormatDate(new Date(time), 'hh:mm')
    }
  },
  methods: {
    timePointFun1 (arr) {
      let date = FormatDate(new Date(new Date().getTime()), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      console.log(date)
      let nowTime = new Date().getTime()
      let timePoint1 = []
      // 空闲的时间段
      for (let i = 0; i <= 8; i++) {
        let hour = (i + 8) < 10 ? '0' + (i + 8) : (i + 8)
        let time = new Date(date + hour + ':00').getTime()
        timePoint1.push({
          time: time,
          type: 0
        })
        // 占用的时间段
        for (let j in arr) {
          let start = arr[j].startTime
          let end = start + (arr[j].durationSeconds * 60 * 1000)
          if (time >= start && time <= end) {
            timePoint1[i].type = 1
          }
        }
        // 过去的时间段
        if (time < nowTime) {
          timePoint1[i].type = 2
        }
      }
      return timePoint1
    },
    timePointFun2 (arr) {
      let date = FormatDate(new Date(new Date().getTime()), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      let nowTime = new Date().getTime()
      let timePoint2 = []
      for (let i = 0; i <= 7; i++) {
        let time = new Date(date + (i + 16) + ':00').getTime()
        timePoint2.push({
          time: time,
          type: 0
        })
        for (let j in arr) {
          let start = arr[j].startTime
          let end = start + (arr[j].durationSeconds * 60 * 1000)
          if (time >= start && time <= end) {
            timePoint2[i].type = 1
          }
        }
        if (time < nowTime) {
          timePoint2[i].type = 2
        }
      }
      return timePoint2
    },
    timeLineFun1 (arr) {
      let date = FormatDate(new Date(new Date().getTime()), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      let startTime = new Date(date + ' 08:00').getTime()
      let minTime = new Date(date + ' 16:00').getTime()
      let nowTime = new Date().getTime()
      let timeLine = []
      // 占用
      for (let i in arr) {
        let width = arr[i].durationSeconds / 60
        let start = arr[i].startTime
        let end = start + (arr[i].durationSeconds * 60 * 1000)
        let left = (start - startTime) / 60 / 1000 / 60
        if (left < 8) {
          if (end > minTime) { // 16 ~ 23
            let d1 = (minTime - arr[i].startTime) / 60 / 1000 // 开始时间到16：00的时常
            let st1 = arr[i].startTime
            let w1 = d1 / 60
            let lf1 = (Math.abs(st1 - startTime) / 60 / 1000 / 60) * 11
            timeLine.push({
              width: w1 * 11 + '%',
              left: lf1 + '%'
            })
          } else { // 8~16
            timeLine.push({
              width: width * 11 + '%',
              left: (Math.abs(start - startTime) / 60 / 1000 / 60) * 11 + '%'
            })
          }
        }
      }
      if (startTime > nowTime) {
        return timeLine
      }
      if (nowTime < minTime) {
        let width = (nowTime - startTime) / 60 / 1000 / 60 * 11
        let left = 0
        timeLine.push({
          width: width + '%',
          left: left + '%',
          border: true
        })
      } else {
        let width = (minTime - startTime) / 60 / 1000 / 60 * 11
        let left = 0
        timeLine.push({
          width: width + '%',
          left: left + '%',
          border: true
        })
      }
      return timeLine
    },
    timeLineFun2 (arr) {
      let date = FormatDate(new Date(new Date().getTime()), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      let startTime = new Date(date + ' 08:00').getTime()
      let timeLine = []
      let nowTime = new Date().getTime()
      let minTime = new Date(date + ' 16:00').getTime()
      for (let i in arr) {
        let width = arr[i].durationSeconds / 60
        let start = arr[i].startTime
        let left = (start - startTime) / 60 / 1000 / 60
        if (left >= 8) {
          timeLine.push({
            width: width * 11 + '%',
            left: (((start - startTime) / 60 / 1000 / 60) - 8) * 11 + '%'
          })
        }
      }
      if (startTime > nowTime) {
        return timeLine
      }
      if (nowTime >= minTime) {
        let width = (nowTime - minTime) / 60 / 1000 / 60 * 11
        let left = 0
        timeLine.push({
          width: width + '%',
          left: left + '%',
          border: true
        })
      }
      return timeLine
    }
  },
  mounted () {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/less" lang="less" scoped>
  .hello{
  .con{
    margin-bottom: 0.47rem;
  .lineTitle{
    width: 100%;
    margin-bottom: 0.26rem;
  .line{
    font-size: 0.3rem;
    width: 11%; float: left;
  }
  }
  .linePoint{
    height: 0.05rem;
    width: 100%;
    position: relative;
    margin-left: 0.44rem;
    z-index: 1;
  .points{
    height: 100%;
    width: 100%;
    display: flex;
  }
  .point{
    /*width: 1.1rem;*/
    width: 11%;
    z-index: 1;
    border-top: 1px solid #19cbab;
  .circular{
    position: relative;
    width: 0.3rem;
    height: 0.3rem;
    /*border-radius: 50%;
    background-color: #19cbab;*/
    img{
      width: 0.3rem;
      width: 0.3rem;
      position: absolute;
      z-index: 200;
      top: -0.15rem;
      left: -0.11rem;
    }
    /*box-shadow: #19cbab 0px 0px 0.3rem;*/
  }
  .bgRed{
    background-color: #19cbab!important;
  }
  .bgBlack{
    background-color: black!important;
  }
  /*position: absolute;
  width: 0.2rem;
  height: 0.2rem;
  z-index: 99;
  border-radius: 50%;
  background-color: #19cbab;*/
  }
  .point:nth-last-child(1){
    border-top: 1px solid transparent;
  }
  .points:nth-of-type(1){
    .point{
     /* margin-top: 0.01rem;*/
    }
  }
  }
  }
  }
</style>
