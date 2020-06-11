<template>
    <div class="deploymentManagement">
        <div class="head">
            <div class="demo-input-suffix">
                <span class="span-font">项目名称：</span>
                <el-input placeholder="请输入项目名称"
                          prefix-icon="el-icon-search"
                          v-model="headForm.name">
                </el-input>
                <span class="span-font">文件版本：</span>
                <el-input placeholder="请输入文件版本"
                          prefix-icon="el-icon-search"
                          v-model="headForm.version">
                </el-input>
                <span class="span-font">作业名称：</span>
                <el-input placeholder="请输入作业名称"
                          prefix-icon="el-icon-search"
                          v-model="headForm.homework">
                </el-input>
                <span class="span-font">审批状态：</span>
                <el-select v-model="headForm.approvalStatus"
                           style="float:left;"
                           placeholder="请选择">
                    <el-option v-for="item in optionsApproval"
                               :key="item.value"
                               :label="item.label"
                               :value="item.value">
                    </el-option>
                </el-select>
                <span class="span-font">操作状态：</span>
                <el-select v-model="headForm.operatingStatus"
                           style="float:left;"
                           placeholder="请选择">
                    <el-option v-for="item in optionsOperating"
                               :key="item.value"
                               :label="item.label"
                               :value="item.value">
                    </el-option>
                </el-select>
            </div>
        </div>
        <div class="content">
            <div class="content-btn">
                <el-button type="primary"
                           @click="addFun">新增作业部署</el-button>
            </div>
            <div class="tab-btn">
                <div class="tab-sigal"
                     @click="checkBtn(1)"
                     :class="activeBtn ? 'active-tab':''">
                    STG
                </div>
                <div class="tab-sigal"
                     @click="checkBtn(2)"
                     :class="!activeBtn ? 'active-tab':''">
                    PROD
                </div>
            </div>
            <el-table ref="singleTable"
                      :data="tableData"
                      highlight-current-row
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
        <el-dialog title="作业部署新增"
                   :visible.sync="dialogFormVisible">
            <el-form :model="form"
                     class="dialog-form">
                <div>
                    <el-form-item label="项目名称："
                                  :label-width="formLabelWidth">
                        <el-select v-model="form.region"
                                   style="float:left;"
                                   placeholder="请选择项目名称">
                            <el-option label="APP1-nginx"
                                       value="1"></el-option>
                            <el-option label="APP1-apx"
                                       value="2"></el-option>
                        </el-select>
                        <i class="el-icon-warning icon"></i>
                        <span>只能选择有创建权限的项目</span>
                    </el-form-item>
                </div>
                <div>
                    <el-form-item label="版本号："
                                  :label-width="formLabelWidth">
                        <el-select v-model="form.version"
                                   style="float:left;"
                                   placeholder="请选择版本号">
                            <el-option label="12121212"
                                       value="1"></el-option>
                            <el-option label="34556332"
                                       value="2"></el-option>
                        </el-select>
                        <i class="el-icon-warning icon"></i>
                        <span>关联已选择的项目，展示版本号</span>
                    </el-form-item>
                </div>
                <div>
                    <el-form-item label="作业名称："
                                  :label-width="formLabelWidth">
                        <el-input v-model="form.homework"
                                  autocomplete="off"></el-input>
                    </el-form-item>
                </div>
                <div>
                    <el-form-item label="作业描述："
                                  :label-width="formLabelWidth">
                        <el-input type="textarea"
                                  maxlength="50"
                                  :autosize="{ minRows: 4, maxRows: 8}"
                                  style="width:400px;float:left;"
                                  show-word-limit
                                  v-model="form.desc"></el-input>
                        <i class="el-icon-warning icon"></i>
                        <span>限制50个字符</span>
                    </el-form-item>
                </div>
                <!-- 选择 -->
                <div>
                    <el-form-item label="环境类型："
                                  :label-width="formLabelWidth">
                        <el-radio v-model="radio"
                                  label="1">STG</el-radio>
                        <el-radio v-model="radio"
                                  label="2">PROD</el-radio>
                    </el-form-item>
                </div>
            </el-form>
            <div slot="footer"
                 class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary"
                           @click="dialogFormVisible = false">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
    export default {
        name: "one",
        data() {
            return {
                radio: '1',
                activeBtn: true,
                // 头部筛选条件
                headForm: {
                    name: '',
                    version: '',
                    homework: '',
                    approvalStatus: '',
                    operatingStatus: ''
                },
                formList: [1],
                form: {
                    region: '',
                    homework: '',
                    version: '',
                    desc: ''
                },
                formLabelWidth: '120px',
                formLabelWidth1: '110px',
                dialogFormVisible: false,
                currentPage: 1,
                optionsApproval: [{
                    value: '1',
                    label: '全部'
                }, {
                    value: '2',
                    label: '成功'
                }, {
                    value: '3',
                    label: '失败'
                }],
                optionsOperating: [{
                    value: '1',
                    label: '全部'
                }, {
                    value: '2',
                    label: '成功'
                }, {
                    value: '3',
                    label: '失败'
                }],
                tableData: [{
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
                },
                {
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
                currentRow: null
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
            // 切换按钮
            checkBtn(val) {
                val == 1 ? this.activeBtn = true : this.activeBtn = false
            },
            setCurrent(row) {
                this.$refs.singleTable.setCurrentRow(row);
            },
            handleCurrentChange(val) {
                this.currentRow = val;
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },
            seeRow(index, row) {
                this.$emit('checkFun', row)
            },
            addFun() {
                this.dialogFormVisible = true
            },
            addForm() {
                this.formList.push(1)
            }
        }
    }
</script>
<style lang="scss" scoped>
    /deep/ .head .el-input__inner {
        float: left;
        width: 200px;
    }
    /deep/ .head .el-input {
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
    .icon {
        font-size: 24px;
        margin-top: 8px;
        margin-left: 10px;
        float: left;
        margin-right: 10px;
    }
    .deploymentManagement {
        flex-direction: column;
        display: flex;
        min-height: 100%;
    }
    .head {
        overflow: hidden;
        background: #fff;
        padding: 20px 10px;
    }
    .span-font {
        line-height: 44px;
        display: block;
        float: left;
    }
    .content {
        flex: 1;
        overflow: hidden;
        background: #fff;
        padding: 20px 10px;
        margin-top: 20px;
        padding-bottom: 50px;
        position: relative;
        .pageNumber {
            position: absolute;
            bottom: 10px;
            right: 20px;
        }
        .content-btn {
            margin-bottom: 10px;
        }
        .tab-sigal {
            width: 126px;
            text-align: center;
            height: 40px;
            line-height: 40px;
            float: left;
            border: 1px solid #dcdfe6;
            cursor: pointer;
        }
        .tab-sigal:nth-child(1) {
            border-right: none;
        }
        .active-tab {
            color: #fff;
            background: #409eff;
        }
    }
    .border-box {
        padding: 10px;
        border: 1px solid #dcdfe6;
        max-height: 300px;
        overflow: auto;
        .bottom-box {
            margin-bottom: 10px;
        }
    }
</style>