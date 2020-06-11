<template>
    <div class="deploymentManagement">
        <div class="head">
            <div class="demo-input-suffix">
                <span class="span-font">项目名称：</span>
                <el-input placeholder="请输入内容"
                          prefix-icon="el-icon-search"
                          v-model="input2">
                </el-input>

                <span class="span-font">操作状态：</span>
                <el-select v-model="value"
                           placeholder="请选择">
                    <el-option v-for="item in options"
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
                           @click="addFun">配置文件新增</el-button>
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
                                 label="配置文件数"
                                 width="180">
                </el-table-column>
                <el-table-column property="id"
                                 align='center'
                                 label="版本编号">
                </el-table-column>
                <el-table-column property="time"
                                 align='center'
                                 label="文件操作时间">
                </el-table-column>
                <el-table-column label="操作"
                                 align='center'
                                 width="120">
                    <template slot-scope="scope">
                        <el-button @click.native.prevent="seeRow(scope.$index, scope.row)"
                                   type="text"
                                   size="small">
                            文件查看
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
        <el-dialog title="文件新增"
                   width='70%'
                   :visible.sync="dialogFormVisible">
            <el-form :model="form"
                     class="dialog-form">
                <div>
                    <el-form-item label="项目名称："
                                  :label-width="formLabelWidth">
                        <el-select v-model="form.region"
                                   placeholder="请选择项目名称">
                            <el-option label="APP1-nginx"
                                       value="1"></el-option>
                            <el-option label="APP1-apx"
                                       value="2"></el-option>
                        </el-select>
                    </el-form-item>
                </div>
                <div>
                    <el-form-item label="Git路径："
                                  :label-width="formLabelWidth">
                        <el-input v-model="form.path"
                                  autocomplete="off"></el-input>
                    </el-form-item>
                    <el-form-item label="版本编号："
                                  :label-width="formLabelWidth">
                        <el-input v-model="form.version"
                                  autocomplete="off"></el-input>
                    </el-form-item>
                </div>
                <div>
                    <el-form-item label="配置文件："
                                  :label-width="formLabelWidth">
                        <div class="border-box">
                            <div v-for="(item,index) in formList"
                                 class="bottom-box"
                                 :key="index">
                                <el-form-item label="配置文件名称："
                                              :label-width="formLabelWidth1">
                                    <el-input v-model="form.configName"
                                              autocomplete="off"></el-input>
                                </el-form-item>
                                <el-form-item label="配置文件路径："
                                              :label-width="formLabelWidth1">
                                    <el-input v-model="form.configPath"
                                              autocomplete="off"></el-input>
                                </el-form-item>
                                <el-form-item label="配置文件："
                                              :label-width="formLabelWidth1">
                                    <el-input v-model="form.config"
                                              autocomplete="off"></el-input>
                                </el-form-item>
                                <el-button type="primary">上传<i class="el-icon-upload el-icon--right"></i></el-button>
                                <el-button type="primary"
                                           icon="el-icon-delete"
                                           @click="deleteFrom(index)"></el-button>
                            </div>

                        </div>
                    </el-form-item>
                </div>
                <!-- 按钮 -->
                <div>
                    <el-form-item label=""
                                  :label-width="formLabelWidth">
                        <el-button type="primary"
                                   @click="addForm">新增配置文件</el-button>
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

        <el-dialog title="文件查看"
                   :visible.sync="dialogFormVisibleSee">
            <el-form :model="form"
                     class="dialog-form">
                <div>
                    <el-form-item label="项目名称："
                                  :label-width="formLabelWidth">
                        <el-input placeholder="请输入内容"
                                  v-model="currentProject"
                                  :disabled="true">
                        </el-input>
                    </el-form-item>
                    <el-form-item label="版本号："
                                  :label-width="formLabelWidth">
                        <el-select v-model="seeForm.version"
                                   placeholder="请选择版本号">
                            <el-option label="APP1-nginx"
                                       value="1"></el-option>
                            <el-option label="APP1-apx"
                                       value="2"></el-option>
                        </el-select>
                    </el-form-item>
                </div>
                <div class="bottom-content">
                    <p>配置文件</p>
                    <el-tabs v-model="activeName"
                             type="card"
                             @tab-click="handleClick">
                        <el-tab-pane label="AR"
                                     name="first">
                            <el-input type="textarea"
                                      :rows="6"
                                      placeholder="请输入内容"
                                      v-model="seeForm.textarea1">
                            </el-input>
                        </el-tab-pane>
                        <el-tab-pane label="NB"
                                     name="second">
                            <el-input type="textarea"
                                      :rows="6"
                                      placeholder="请输入内容"
                                      v-model="seeForm.textarea2">
                            </el-input>
                        </el-tab-pane>
                        <el-tab-pane label="AC"
                                     name="third">
                            <el-input type="textarea"
                                      :rows="6"
                                      placeholder="请输入内容"
                                      v-model="seeForm.textarea3">
                            </el-input>
                        </el-tab-pane>
                    </el-tabs>
                </div>

            </el-form>
            <div slot="footer"
                 class="dialog-footer">
                <el-button @click="dialogFormVisibleSee = false">取 消</el-button>
                <el-button type="primary"
                           @click="dialogFormVisibleSee = false">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
    export default {
        name: "deploymentManagement",
        data() {
            return {
                activeName: 'first',
                currentProject: '',
                dialogFormVisibleSee: false,
                formList: [1],
                seeForm: {
                    version: '',
                    textarea1: '',
                    textarea2: '',
                    textarea3: ''
                },
                form: {
                    region: '',
                    path: '',
                    version: '',
                    configName: '',
                    configPath: '',
                    config: ''
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
            // tab标签页点击
            handleClick(tab, event) {
                console.log(tab, event);
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
                console.log(row)
                this.currentProject = row.name
                this.dialogFormVisibleSee = true
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
    .bottom-content {
        box-sizing: border-box;
        padding: 10px 40px;
        p {
            margin-bottom: 20px;
            font-weight: bold;
            font-size: 18px;
        }
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