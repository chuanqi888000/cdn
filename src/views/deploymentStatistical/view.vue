<template>
    <div>
       <div class="block">
    <p class="demonstration1">部署统计</p>

    <div class="time">
        <span class="demonstration">部署时间</span>
        <el-date-picker
          v-model="value1"
          type="datetimerange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期">
        </el-date-picker>
           <el-button type="primary" @click="onSubmit">查询</el-button>
    </div>
    
    <el-table ref="singleTable"
                      :data="tableData"
                      highlight-current-row
                      @current-change="handleCurrentChange"
                      style="width: 100%">
                <el-table-column property="name"
                                 align='center'
                                 label="项目名">
                </el-table-column>
                <el-table-column property="number"
                                 align='center'
                                 label="Stg发布次数"
                                 width="180">
                </el-table-column>
                <el-table-column property="id"
                                 align='center'
                                 label="Prod发布次数">
                </el-table-column>
                <el-table-column property="time"
                                 align='center'
                                 label="项目发布总计">
                </el-table-column>
    </el-table>
  </div>
    </div>
</template>
<script>
    export default {
        name: "deploymentStatistical",
        data() {
            return {
                form: {
                name: '',
                region: '',
                date1: '',
                date2: '',
                delivery: false,
                type: [],
                resource: '',
                desc: '',
        },
          tableData: [{
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                }, {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                }, {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
      ],
    }}
    }
</script>
<style lang="scss" scoped>
 .time{
     background: #fff;
    padding: 20px 10px;
     margin: 0 auto;
     margin-bottom: 20px;
 }
 .demonstration,.demonstration1{
    background: #fff;  
 }
 .demonstration1{
    font-size: 18px;
    padding: 10px;
 }

</style>