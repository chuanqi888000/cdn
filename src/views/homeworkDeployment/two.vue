<template>
    <div class="two">
        <div class="head">
            <el-button type="primary"
                       class="return"
                       @click="returnFun">返回</el-button>
            <el-table :data="tableData"
                      :show-header="headerTab"
                      border
                      style="width: 100%">
                <el-table-column prop="one"
                                 label=""
                                 width="100">
                </el-table-column>
                <el-table-column prop="two"
                                 label="">
                </el-table-column>
                <el-table-column prop="three"
                                 label=""
                                 width="100">
                </el-table-column>
                <el-table-column prop="four"
                                 label="">
                </el-table-column>
                <el-table-column prop="five"
                                 label=""
                                 width="100">
                </el-table-column>
                <el-table-column prop="six"
                                 label="">
                </el-table-column>
                <el-table-column prop="seven"
                                 label=""
                                 width="100">
                </el-table-column>
                <el-table-column prop="eight"
                                 label="">
                </el-table-column>

            </el-table>
        </div>
        <div class="content">
            <el-tabs v-model="activeName"
                     type="card"
                     @tab-click="handleClick">
                <el-tab-pane label="执行日志"
                             name="first">

                </el-tab-pane>
            </el-tabs>
            <div class="search">
                <el-input placeholder="请输入搜索内容"
                          prefix-icon="el-icon-search"
                          v-model="headForm.name">
                </el-input>
                <el-button type="primary"
                           icon="el-icon-search">搜索日志</el-button>
            </div>
            <div class="bottom-content">
                <div class="left-content">
                    <div class="demo-input-suffix">
                        <span class="span-font">ip过滤：</span>
                        <el-input placeholder="请输入ip过滤"
                                  prefix-icon="el-icon-search"
                                  v-model="headForm.name">
                        </el-input>
                    </div>

                    <el-table ref="singleTable"
                              :data="tableData1"
                              highlight-current-row
                              height="300px"
                              @current-change="handleCurrentChange"
                              style="width: 100%">
                        <el-table-column property="name"
                                         align='center'
                                         label="项目名称">
                        </el-table-column>
                        <el-table-column property="id"
                                         align='center'
                                         label="配置文件版本"
                                         width="180">
                        </el-table-column>
                        <el-table-column property="workName"
                                         align='center'
                                         label="作业名称">
                        </el-table-column>
                        <el-table-column property="desc"
                                         align='center'
                                         label="作业描述">
                        </el-table-column>
                        <el-table-column property="step"
                                         align='center'
                                         label="执行总进度">
                        </el-table-column>
                        <el-table-column prop="status"
                                         align='center'
                                         label="操作状态">
                            <template slot-scope="scope">
                                <span>{{operating(scope.row.status)}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column label="操作"
                                         align='center'
                                         width="120">
                            <template slot-scope="scope">
                                <el-button @click.native.prevent="seeRow(scope.$index, scope.row)"
                                           type="text"
                                           size="small">
                                    查看日志
                                </el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                    <el-pagination @size-change="handleSizeChange"
                                   @current-change="handleCurrentChange"
                                   :current-page="currentPage"
                                   :page-sizes="[100, 200, 300, 400]"
                                   :page-size="100"
                                   layout="total, sizes, prev, pager, next, jumper"
                                   :total="400"
                                   class="pageNumber">
                    </el-pagination>
                </div>
                <div class="left-content"
                     style="padding-left:10px;">
                    <el-checkbox-group v-model="checkList">
                        <el-checkbox label="完整日志"
                                     disabled></el-checkbox>
                        <el-checkbox label="暂停查询"></el-checkbox>
                        <el-input type="textarea"
                                  style="background:#000;"
                                  :rows="16"
                                  placeholder="请输入内容"
                                  v-model="headForm.textarea">
                        </el-input>
                    </el-checkbox-group>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: "two",
        data() {
            return {
                checkList: ['完整日志'],
                currentPage: 1,
                activeName: 'first',
                headerTab: false,
                headForm: {
                    name: '',
                    textarea: ''
                },
                tableData: [{
                    one: '任务名称',
                    two: 'ap_1212_wuewi_89394',
                    three: '项目名称',
                    four: 'app-nginx部署',
                    five: '版本编号',
                    six: 'v1版本',
                    seven: '总时间（s）',
                    eight: '40.92'
                }, {
                    one: '执行状态',
                    two: '',
                    three: '开始时间',
                    four: '2014-02-18 12:01:33',
                    five: '结束时间',
                    six: '2019-02-18 12:01:33',
                    seven: '',
                    eight: ''
                }],
                tableData1: [{
                    desc: '作业是正常状态的',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    workName: '2020app1作业下发1',
                    status: 0,
                    step: '10%'
                }, {
                    desc: '作业是正常状态的',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    workName: '2020app1作业下发1',
                    status: 0,
                    step: '10%'
                }, {
                    desc: '作业是正常状态的',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    workName: '2020app1作业下发1',
                    status: 0,
                    step: '10%'
                },
                {
                    desc: '作业是正常状态的',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    workName: '2020app1作业下发1',
                    status: 0,
                    step: '10%'
                }
                ],
            }
        },
        computed: {
            operating(val) {
                return function (val) {
                    return val == 0 ? '审批中' : '驳回中'
                }
            }
        },
        methods: {
            returnFun() {
                this.$emit('twoCheckFun', false)
            },
            handleClick(tab, event) {
                console.log(tab, event);
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },
            handleCurrentChange(val) {
                this.currentRow = val;
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
        }
    }
</script>
<style lang="scss" scoped>
    /deep/ .bottom-content .demo-input-suffix .el-input__inner {
        float: left;
        width: 200px;
    }
    /deep/ .search .el-input__inner {
        float: left;
        width: 92%;
    }
    /deep/ .search .el-input {
        width: 92%;
        float: left;
        margin-right: 10px;
    }
    /deep/ .bottom-content .demo-input-suffix .el-input {
        width: 200px;
        float: left;
        margin-right: 30px;
    }
    /deep/ .dialog-form .el-input__inner {
        width: 200px;
    }
    /deep/ .el-form-item {
        display: inline-block;
    }
    /deep/ .dialog-form .border-box .el-input__inner {
        width: 70px;
    }
    .span-font {
        line-height: 44px;
        display: block;
        float: left;
    }
    .two {
        flex-direction: column;
        display: flex;
        min-height: 100%;
    }
    .head {
        overflow: hidden;
        background: #fff;
        padding: 20px 10px;
    }
    .return {
        float: right;
        margin-bottom: 20px;
    }
    .content {
        flex: 1;
        overflow: hidden;
        background: #fff;
        padding: 20px 10px;
        margin-top: 20px;
        position: relative;
        flex-direction: column;
        display: flex;
        .bottom-content {
            flex: 1;
            margin-top: 20px;
            .left-content {
                float: left;
                width: 50%;
                box-sizing: border-box;
                height: 100%;
            }
        }
    }
</style>