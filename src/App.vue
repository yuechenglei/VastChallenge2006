<template>
  <div id="app">
    <i-menu mode="horizontal" theme="dark" active-name="1">
      <menu-item name="1">
        VAST Challenge 2006
      </menu-item>
    </i-menu>
    <div id="content">
      <myHeader id="zoneL"></myHeader>
      <div id="zoneM">
        <netWork id="network"></netWork>
        <timeLine id="timeline"></timeLine>
      </div>
      <div id="zoneR">
        <myIndex id="text"></myIndex>
        <geoTime id="geotime"></geoTime>
      </div>
      <!-- <myContent></myContent> -->
      <!-- <router-view/> -->
    </div>
  </div>
</template>
<script>
import myHeader from './components/header.vue'
import myFooter from './components/map.vue'
import myIndex from './components/index.vue'
import timeLine from './components/timeline.vue'
import geoTime from './components/geotime.vue'
import netWork from './components/network.vue'
import * as d3 from 'd3-3'

export default {
  name: 'app',
  components: { myHeader, myFooter, myIndex, timeLine, geoTime, netWork },
  mounted() {
    ///////////////load data before render
    // d3.json("../static/data.json", function(error, data) {
    //   if (error) throw error;

    //   // Load the CSV data
    //   console.log(data)
    // })
  },
  async created() {
    var self = this;
    this.$Loading.start();

    var csv = d3.dsv(",", "text/csv;charset=gb2312");

    await csv("../static/news-original-and-extra-raw-tables/City Hall Phone Log/Conference Room Phone Log.CSV", function(error, data) {
      if (error) {
        self.$Loading.error()
        throw error;
      }
      // Load the json data
      // self.sleep(1000)
      console.log(data[0])
    })

    await csv("../static/news-original-and-extra-raw-tables/Voter Registry/Voter Registry.csv", function(error, data) {
      if (error) {
        self.$Loading.error()
        throw error;
      }
      // Load the CSV data
      // self.sleep(1000)

      var groupData = d3.nest()
        .key(function(d) { return d.SEX; })
        .entries(data);

      console.log(groupData)
    })

    this.$Loading.finish();
  },

  methods: {
    sleep(time) {
      var now = new Date();
      var exitTime = now.getTime() + time;
      while (true) {
        now = new Date();
        if (now.getTime() > exitTime)
          return;
      }
    }
  }
}

</script>
<style lang="less">
@import "./style/base/base.vars.less";
@import "./style/base/style.less";
@import "./style/base/iview.less";
#app {
  position: absolute;
  height: 100%;
  width: 100%;

  #content {
    position: absolute;
    width: 100%;
    height: calc(~"100% - 50px");

    #zoneL {
      position: absolute;
      left: 0;
      top: 0;
      width: 15%;
      height: 100%;
      background: @color-bd;
    }
    #zoneM {
      position: absolute;
      left: 15%;
      top: 0;
      width: 55%;
      height: 100%; //background: @color-main;
      #network {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 75%;
      }

      #timeline {
        position: absolute;
        left: 0;
        top: 75%;
        width: 100%;
        height: 25%;
        background: @color-bd;
      }
    }
    #zoneR {
      position: absolute;
      top: 0;
      left: 70%;
      width: 30%;
      height: 100%;


      #text {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 55%;
         background: @color-bd;
      }

      #geotime {
        position: absolute;
        left: 0;
        top: 55%;
        width: 100%;
        height: 45%;
       
      }
    }
  }
}

</style>
