<template>
    <div class="deploymentManagement">
        <div class="head">
            <div class="demo-input-suffix">
                <span class="span-font">项目列表</span>
                <el-input placeholder="请输入内容"
                          prefix-icon="el-icon-search"
                          class="qwer"
                          v-model="input2">
                </el-input>
            </div>
        </div>
        <div class="content">
            <div class="content-btn">
                <el-button type="primary"
                           @click="addFun">新建项目</el-button>
                <el-button type="primary"
                           @click="addFun1">导出</el-button>
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
                <el-table-column property="number"
                                 align='center'
                                 label="项目描述"
                                 width="180">
                </el-table-column>
                <el-table-column property="id"
                                 align='center'
                                 label="关联CMDB模型">
                </el-table-column>
                <el-table-column property="time"
                                 align='center'
                                 label="GIT路径">
                </el-table-column>
                <el-table-column label="操作"
                                 align='center'
                                 width="160">
                    <template slot-scope="scope">
                        <el-button @click.native.prevent="seeRow(scope.$index, tableData)"
                                   type="text"
                                   size="small">
                            复制
                        </el-button>
                        <el-button @click.native.prevent="seeRow(scope.$index, tableData)"
                                   type="text"
                                   size="small">
                            编辑
                        </el-button>
                        <el-button @click.native.prevent="seeRow(scope.$index, tableData)"
                                   type="text"
                                   size="small">
                            删除
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
        <el-dialog title="新增项目"
                   :visible.sync="dialogFormVisible">
           <el-form ref="form" :model="form" label-width="80px">        
              <el-form-item label="项目名称：" label-width="180px">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="关联CMDB项目：" class="text-label"  label-width="180px">
                <el-select v-model="form.region" placeholder="请选择关联" >
                <el-option label="项目一" value="shanghai" style="width:180px"></el-option>
                <el-option label="项目二" value="beijing"></el-option>
              </el-select>
             </el-form-item>
             <el-form-item label="活动形式：" style="margin-left:88px" label-width="92px">
                <el-input type="textarea" v-model="form.desc" style="width:654px;"></el-input>
             </el-form-item>
             <el-form-item label="GIT地址：" style="margin-left:88px" label-width="92px">
                <el-input type="text" v-model="form.desc" style="width:654px;"></el-input>
             </el-form-item>
             <el-form-item label="GIE用户：" label-width="180px">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="GIE密码：" label-width="180px">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="本地下载路径：" label-width="212px">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="目标上传路径：" label-width="184px">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="执行前置任务：" label-width="180px">
                <el-select v-model="form.region" placeholder="请选择关联" >
                <el-option label="项目一" value="shanghai" style="width:180px"></el-option>
                <el-option label="项目二" value="beijing"></el-option>
              </el-select>
              </el-form-item>
              <el-form-item label="前置任务参数："label-width="180px">
                <el-input v-model="form.name"></el-input>
              </el-select>
              </el-form-item>
              <el-form-item label="执行后置任务：" label-width="180px">
                <el-select v-model="form.region" placeholder="请选择关联" >
                <el-option label="项目一" value="shanghai" style="width:180px"></el-option>
                <el-option label="项目二" value="beijing"></el-option>
              </el-select>
              </el-form-item>
              <el-form-item label="后置任务参数："label-width="180px">
                <el-input v-model="form.name"></el-input>
              </el-select>
              </el-form-item>
           </el-form> 
           <span slot="footer" class="dialog-footer">
               <el-button @click="centerDialogVisible = false">取 消</el-button>
               <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
           </span>
        </el-dialog>
    </div>
</template>
<script>
    export default {
        name: "projectManagement",
        data() {
            return {
                formList: [1],
                form: {
                    region: '',
                    path: '',
                    version: '',
                    configName: '',
                    configPath: '',
                    config: '',
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    delivery: false,
                    type: [],
                    resource: '',
                    desc: ''
                },
                formLabelWidth: '120px',
                formLabelWidth1: '110px',
                dialogFormVisible: false,
                currentPage: 1,
                options: [{
                    value: '1',
                    label: '全部'
                }, {
                    value: '2',
                    label: '成功'
                }, {
                    value: '3',
                    label: '失败'
                }],
                value: '',
                input2: '',
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
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                },
                {
                    time: '2020-05-25 14:17:20',
                    name: 'APP1-nginx',
                    id: '2020052311570809',
                    number: 23
                }
                ],
                currentRow: null
            }
        },
        methods: {
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

            },
            addFun() {
                this.dialogFormVisible = true
            },
            addForm() {
                this.formList.push(1)
            },
            // 删除
            deleteFrom(index) {
                if (this.formList.length == 1) {
                    this.$message({
                        message: '配置文件必须保留一条',
                        type: 'warning'
                    });
                } else {
                    this.formList.splice(index, 1)
                }
            }
        }
    }
</script>
<style lang="scss" scoped>
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
        font-size: 25px;
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
    .head .qwer{
        position: relative;
        left: 450px;
        width: 400px;
    }
    .head .el-input__inner{
        width: 400px;
    }
    .text-label{
        width: 484px;
        float:right;
        label{
            width: 150px;
        }
    }
    .el-form-item__label{
        width: 180px !important;
    }
</style>