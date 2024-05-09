<template>
    <div style="padding: 20px;">

        <p>请选择班级</p>
        <div>
            <el-select v-model="paramData.classId" filterable placeholder="请输入">
                <el-option
                        v-for="item in queryCla"
                        :key="item.classId"
                        :label="item.className"
                        :value="item.classId">
                </el-option>
            </el-select>
        </div>
        <p>请输入补充知识点标题：<el-input style="margin-top: 20px;"
                placeholder="请输入标题"
                v-model="paramData.title"
                clearable>
        </el-input></p>
        正题如下：
        <div style="margin-top: 20px;">
            <mavon-editor
                    :subfield="false"
                    :autofocus="false"
                    v-model="paramData.content"
                    ref="md"
                    style="width: 99%;"
            >
            </mavon-editor>
            <div style="float: right; margin-right: 20px;margin-top: 20px;">
                <router-link to="/personalinfo">
                    <el-button type="info"> 返回</el-button>
                </router-link>
                <el-button @click="addTest(paramData)" style=" margin-left: 20px;"> 提交</el-button>
            </div>
        </div>
    </div>

</template>

<script>
    import {pclass} from "../../api/admin/queryclass";
    import {addKnow,saveTest} from  '../../api/teacher/test.js'
    import Cookies from "js-cookie";
    export default {
        name: "PDetail",
        data(){
            return{
                queryCla:[],
                paramData:{
                    id:'',
                    title:'',
                    classId:'',
                    content:'',
                    creator:'',
                },

                param:{
                    userId:'',
                },
                createTime:''
            }
        },
        computed(){


        },
        created() {
            this.paramData.creator=Cookies.get('userId')
            this.param.userId=Cookies.get('userId')
            this.queryClass(this.param)
            this.paramData.title=this.$route.params.data1.title
            this.paramData.content=this.$route.params.data1.content
            this.paramData.id=this.$route.params.data1.id
            this.createTime=this.$route.params.data1.createTime
            console.log(this.$route.params.data1)
        },
        methods:{
            queryClass(pa){
                pclass(pa).then(resp=>{
                    this.queryCla=resp.data.resultData
                })
            },
            addTest(paramData){

                if(this.createTime){
                    saveTest(paramData).then(resp=>{
                        this.$router.push("/personalinfo")
                        if(resp.data.code==200){

                            this.$message({
                                message: '新增成功',
                                type: 'success'
                            });
                        }else {
                            this.$message.error('发布失败');
                        }
                    })
                } else{

                    addKnow(paramData).then(resp=>{
                        if(resp.data.code==200){
                            this.$router.push("/personalinfo")
                            this.$message({
                                message: '发布成功',
                                type: 'success'
                            });
                            this.$router.push("/personalinfo")


                        }else {
                            this.$message.error('发布失败');
                        }
                    })
                }

                }

        }
    }
</script>

<style scoped>

</style>