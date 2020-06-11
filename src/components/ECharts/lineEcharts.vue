<template>
    <div  class="echad">
      <div :id="id" :style="{width: width, height: height}"></div>
    </div>
</template>

<script>
import echarts from "echarts"
import westeros from "./theme/westeros"
export default {
  name: "lineEcharts",
  props: {
    id: {
      type: String,
      default: "myChart"
    },
    width: {
      type: String,
      default: "100%"
    },
    height: {
      type: String,
      default: "100%"
    }
  },
  data () {
    return {
      chart: null
    }
  },
  mounted () {
    this.initChart()
  },
  methods: {
    initChart () {
      this.chart = echarts.init(document.getElementById(this.id), "westeros")

      this.chart.setOption({
        title: {
          // text: "部署次数总览"
        },
        tooltip: {
          trigger: "axis"
        },
        legend: {
          data: ["邮件营销", "联盟广告", "视频广告", "直接访问", "搜索引擎"]
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: ["周一", "周二", "周三", "周四", "周五", "周六", "周日"]
        },
        yAxis: {
          type: "value"
        },
        series: [
          {
            // name: "邮件营销",
            type: "line",
            stack: "总量",
            data: [8200, 6320, 5010, 4340, 3400, 2300, 1100]
          },
        ]
      })
    }
  }
}
</script>

<style scoped>
 .echad{
   width:100%;
   height:500px;
   background-color:#fff;
 }
</style>
