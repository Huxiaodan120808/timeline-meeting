<template>
  <div class="hello w100">
    <div class="con w100">
      <div class="lineTitle displayflex">
        <div class="line blueFontColor" v-for="(i, index) in timePointFun(msgFather)" :key="index"
             :class="{fontColorCCC: i.type === 2}">
          {{i.time | formatTime1}}
        </div>
      </div>
      <div class="linePoint" id="linePoint">
        <div class="points">
          <!--绿色底 point-->
          <div class="point" v-for="(item, index) in totalBlock" :key="index">
          </div>
        </div>
        <!--橙色占用 灰色不占用-->
        <div class="timePoint"  v-for="(line1, index) in timeLineFun(msgFather)" :key="index" :style="{
        width: line1.width, left: line1.left}" :class="{redBlcak: line1.type === 'CONF_ONLINE', greenBlcak: line1.type === 'CONF_SCHEDULE', darkBlcak: line1.type === 'CONF_OCCUPY', blueBlcak: line1.type === 'CONF_OFFLINE'}"></div>
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
    /*background-color: #feb166;*/
    height: 10px;
  }
  .redBlcak {
    background-color: #ff9499;
    z-index: 99;
  }
  .greenBlcak {
    background-color: #08d182;
    z-index: 99;
  }
  .blueBlcak {
    background-color: #6bddff;
    z-index: 99;
  }
  .darkBlcak {
    background-color: #808080;
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
      timeLine2: [],
      totalBlock: []
    }
  },
  filters: {
    formatTime1 (time) {
      return FormatDate(new Date(time), 'hh')
    }
  },
  methods: {
    timePointFun (arr) {
      let date = FormatDate(new Date(new Date().getTime() + 24 * 60 * 60 * 1000), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      let nowTime = new Date().getTime()
      let timePoint1 = []
      // 空闲的时间段
      for (let i = 0; i <= 10; i++) {
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
    // 计算占用的时间段
    timeLineFun (arr) {
      let date = FormatDate(new Date(new Date().getTime() + 24 * 60 * 60 * 1000), 'yyyy-MM-dd').replace(/-/g, '/') + ' '
      let startTime = new Date(date + ' 08:00').getTime()
      let maxTime = new Date(date + ' 19:00').getTime()
      let timeLine = []
      // 占用
      for (let i in arr) {
        let width = arr[i].durationSeconds / 60
        let start = arr[i].startTime
        let end = start + (arr[i].durationSeconds * 60 * 1000)
        let left = (start - startTime) / 60 / 1000 / 60
        if (left < 20) {
          if (end > maxTime) { // 如果结束时间比当前时间还长就取到结束的时间
            width = (maxTime - start) / 1000 / 60 / 60
            timeLine.push({
              width: width * 10 + '%',
              left: (Math.abs(start - startTime) / 60 / 1000 / 60) * 9 + '%',
              type: arr[i].status
            })
          } else {
            timeLine.push({
              width: width * 9 + '%',
              left: (Math.abs(start - startTime) / 60 / 1000 / 60) * 9 + '%',
              type: arr[i].status
            })
          }
        }
      }
      console.log(timeLine)
      return timeLine
    }
  },
  mounted () {
    let that = this
    // 初始化总共显示的各自数
    for (let j = 0; j <= 21; j++) {
      that.totalBlock.push(j)
    }
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
    margin-bottom: 0.46rem;
  .line{
    font-size: 0.3rem;
    width: 9%;
    float: left;
    text-align: left;
  }
  }
  .linePoint{
    height: 0.05rem;
    width: 100%;
    position: relative;
    z-index: 1;
    /*margin-left: 3%;*/
  .points{
    height: 100%;
    width: 100%;
    display: flex;
  }
  .point{
    width: 9.3%;
    z-index: 1;
    height: 10px;
    background-color: #ddd;
    border-right: 1px solid white;
  .circular{
    position: relative;
    width: 0.5rem;
    height: 0.5rem;
    img{
      width: 0.5rem;
      position: absolute;
      z-index: 200;
      top: -0.25rem;
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
    /*border-top: 1px solid transparent;*/
    /*background-color: transparent;*/
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
