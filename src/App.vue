<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App" style="display: none;"/>
    <button @click="startRecord">开始采集</button>
    <button @click="endRecord">结束采集</button>
    <button @click="replay">回放</button>
    <div>
        <div v-show="isDiv">
            我害怕鬼， 但归为伤我分毫
        </div>
        <button @click="isDivShow">点击展示</button>
    </div>
    <div id="replayContent" style="border: 2px #454545 soild; width: 400px; height: 300px;"> </div>
  </div>
  
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import * as rrweb from "rrweb"
import rrwebPlayer from 'rrweb-player';
import 'rrweb-player/dist/style.css';
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return {
        isDiv: false,
        events : []
    }
  },
  methods:{
    isDivShow() {
        this.isDiv = !this.isDiv
    },
    replay() {
        var that = this
        new rrwebPlayer({
            target: document.getElementById("replayContent"), // 可以自定义 DOM 元素
            // 配置项
            props: {
                events:that.events,
            },
        });
    },
    endRecord() {
        console.log(this.events)
        var that = this
        let stopFn = rrweb.record({
        emit(event) {
            if (that.events.length > 50) {
                // 当事件数量大于 100 时停止录制
                console.log(22222222222, event)
                // stopFn();
            }
        },
        });
        if (that.events.length > 50) {
            // 当事件数量大于 100 时停止录制
            console.log(22222222222, event)
            stopFn();
        }
        
    },
    startRecord() {
        console.log(rrweb)
        var that = this
        rrweb.record({
            emit(event) {
                console.log(event, 5555555, that)
                // 将 event 存入 events 数组中
                that.events.push(event);
                console.log(that.events, 'this.events------------')
            },
        });
        console.log(that.events)
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
