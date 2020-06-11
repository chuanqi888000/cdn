<template>
<div>
    <div class="title-con">
       <div class="title-chid" v-for="(title1,index) in title">
          <img src="../.././assets/logo.png" alt="" style="width:100px; height:100px;background-color:#fff">
          <div class="title-text">
            <p style="line-height:30px;font-size: 16px;">{{title1.text}}</p>
            <p style="font-size: 18px;font-weight: bold;">{{title1.shou}}</p>
          </div>
       </div>
    </div>

  <el-tabs type="border-card">
  <el-tab-pane label="用户管理"></el-tab-pane>
  <el-tab-pane label="配置管理"></el-tab-pane>
  <el-tab-pane label="角色管理"></el-tab-pane>
  <lineEcharts></lineEcharts>
  </el-tabs>

    <div class="foot">
       <div>
         <p style="font-size:20px;margin-bottom:10px">部署成功</p>
         <p style="font-size:18px">0</p>
       </div>
       <div>
         <p style="font-size:20px; color:red;margin-bottom:10px">部署成功</p>
         <p style="font-size:18px">0</p>
       </div>
    </div>
</div>   
</template>

<script>
import LineEcharts from "../../components/ECharts/lineEcharts"
import Maintable from "../table/maintable"
export default {
  name: "mainIndex",
  components: {Maintable, LineEcharts},
  data(){
     return{
       title:[
         {shou:1,text:"部署作业数"},
         {shou:2,text:"待审核作业数"},
         {shou:4,text:"项目数"},
         {shou:5,text:"配置版本数"},
       ]
     }
  },
  mounted () {
    this.selfAdaption()
  },
  methods: {
    
  }
}
</script>

<style lang="scss">
 .title-con{
   display: flex;
   justify-content: space-between;
   margin-bottom: 20px;
 }
 .title-chid{
   display: flex;
 }
 .title-text{
   width: 200px;
   height: 100px;
   background-color: #fff;
   padding-left: 10px;
 }
 .foot{
   padding: 50px 0 ;
   display: flex;
   justify-content: space-around;
   background-color:#fff;
   div{
     width: 50%;
     text-align: center;
   }
 }

</style>
