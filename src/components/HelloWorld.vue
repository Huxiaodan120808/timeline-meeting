<template>
  <div class="hello">
    <div class="w100 tipsBlock">
      <div class="tips"><div class="redBlcak"></div>占用</div>
      <div class="tips"><div class="greenBlcak"></div>已预约</div>
      <div class="tips"><div class="darkBlcak"></div>已完成</div>
      <div class="tips"><div class="orangeBlcak"></div>待审批</div>
      <div class="tips"><div class="blueBlcak1"></div>已申请</div>
    </div>
    <div class="roomList">
      <div class="room w100" v-for="(room, index) in meetingRoom" :key="index">
        <time-block class="roomTime" v-bind:msgFather = "room.occupy"></time-block>
        <div class="w100 subTitle">
          <div class="w50 textLeft">{{room.name}}</div>
          <div class="w50 textRight">10人，电子屏</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TimeBlock from './timeBlockTwoLine'
// import TimeBlock from './timeBlockSingle'
// import TimeBlock from './timeBlockBlock'
// import TimeBlock from './timeBlockBlock2'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      meetingRoom: [{id: 1,
        name: 'room1',
        occupy: [{
          durationSeconds: 50,
          name: 'C樟的会议',
          startTime: 1541205060000,
          status: 'CONF_FINISH',
          userName: 'C樟',
          uuid: '03d299ff-af81-4f33-a97d-045d4cc20f53'}, {
          durationSeconds: 50,
          name: 'FDDS樟的会议',
          startTime: 1541212260000,
          status: 'CONF_OCCUPY',
          userName: '曾C樟',
          uuid: '03d299ff-af81-4f33-a97d-045d4cc20f43'}, {
          durationSeconds: 180,
          name: 'FDDS樟的会议',
          startTime: 1541230200000,
          status: 'CONF_SCHEDULE',
          userName: '曾C樟',
          uuid: '03d299ff-af81-4f33-a97d-045d4cc20f43'}]}, {id: 2,
        name: 'room2',
        occupy: [{
          durationSeconds: 60,
          name: 'DFD的会议',
          startTime: 1541212200000,
          status: 'CONF_OCCUPY',
          userName: 'DFD樟',
          uuid: '03d299ff-af81-4f33-a97d-045d4cc20f53'}, {
          durationSeconds: 90,
          name: 'DFD的会议',
          startTime: 1541219400000,
          status: 'CONF_APPROVAL',
          userName: 'DFD樟',
          uuid: '03d299ff-af81-4f33-a97d-023d4cc20f53'}, {
          durationSeconds: 90,
          name: 'DFD的会议',
          startTime: 1541226600000,
          status: 'CONF_APPLY',
          userName: 'DFD樟',
          uuid: '03d299ff-af81-4f33-a97d-023d4cc20f53'}, {
          durationSeconds: 80,
          name: 'DFD的会议',
          startTime: 1541237400000,
          status: 'CONF_OCCUPY',
          userName: 'DFD樟',
          uuid: '03d299ff-af81-4f33-a97d-023d4cc20f53'}]}]
    }
  },
  components: {
    TimeBlock
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  rel="stylesheet/less" lang="less" scoped>
  .hello{
    .tipsBlock{
      width: 100%;float: left;
      height: 30px;
      .tips{width: 20%;float: left;line-height: 30px;font-size: 14px;position: relative;color: #808080;
      div{width: 10px;height: 10px;border-radius: 2px;float: left;margin-top: 10px;}}
      .orangeBlcak {
        background-color: #feb166 !important;
        z-index: 99;
      }
      .redBlcak {
        background-color: #ff9499 !important;
        z-index: 99;
      }
      .greenBlcak {
        background-color: #08d182!important;
        z-index: 99;
      }
      .blueBlcak {
        background-color: #6bddff!important;
        z-index: 99;
        /*width: 20px !important;*/
        border: 1px solid #ddd;
        height: 10px !important;
        display: block!important;
      }
      .blueBlcak:after {
        content: ' ';
        display: inline-block;
        border: 2px solid #fff;
        border-top-width: 0;
        border-right-width: 0;
        width: 8px;
        height: 4px;
        -webkit-transform: rotate(-50deg);
        -moz-transform: rotate(-50deg);
        -ms-transform:rotate(-50deg);
        position: absolute;
        top: 0;
        left: 2px;
      }
      .blueBlcak1 {
        background-color: #6bddff!important;
        z-index: 99;
        /*width: 20px !important;*/
        border: 1px solid #ddd;
        height: 10px !important;
        display: block!important;
      }
      .blueBlcak1:after {
        content: ' ';
        display: inline-block;
        border: 2px solid #fff;
        border-top-width: 0;
        border-right-width: 0;
        width: 6px;
        height: 3px;
        -webkit-transform: rotate(-50deg);
        -moz-transform: rotate(-50deg);
        -ms-transform:rotate(-50deg);
        position: absolute;
        top: 12px;
        left: 2px;
      }
      .darkBlcak {
        background-color: #808080!important;
        z-index: 99;
      }
      .fontColorCCC {
        color: #808080;
      }
    }
    .roomList{position: absolute;
      top: 100px;height: auto;width: 96%}
  }
</style>
<style  scoped>
.subTitle{color: #ccc;font-size: 12px;padding-top: 5px;}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
  .room{
    border-bottom: 1px solid #ddd;
    height: 50px;
    margin-bottom: 5px;
    padding-bottom: 10px;}
 .w100{width: 100%;}
</style>
