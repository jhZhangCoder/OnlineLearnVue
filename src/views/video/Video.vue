<!--
 * @Description: 
 * @Author: Rabbiter
 * @Date: 2023-05-15 23:22:25
-->
<template>
    <div style="overflow:auto;height: 480px;">
        
        
        <el-dialog title="请输入视频集的内容" :visible.sync="goDealDialogVisible" width="50%">
            <template>
                <div>
                    <div style="margin: 10px;"></div>
                    <el-form label-width="100px">
                        <el-form-item label="视频集标题">
                            <el-input v-model="uploadVideo.topic" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item label="上传图片">
                            <el-upload class="upload-demo" ref="upload" :data="uploadVideo"
                                action="http://127.0.0.1:9251/study/videoTotal/save" :on-preview="handlePreview"
                                :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3"
                                :on-exceed="handleExceed" :file-list="uploadVideo.fileList">
                                <el-button size="small" type="info"> 上传封面</el-button>
                                <div slot="tip" class="el-upload__tip">只能上传jpg/png文件</div>
                            </el-upload>
                        </el-form-item>



                    </el-form>
                </div>
            </template>

            <span slot="footer" class="dialog-footer">
                <el-button @click="goDealDialogVisible = false"> 取 消</el-button>
                <el-button type="info" @click="submit(videoParam)"> 确 定</el-button>
            </span>
        </el-dialog>


        <el-button  @click="uploadddVideo()" style="float: right;margin-bottom: 10px; margin-right: 40px;"> 新增教学视频集</el-button>
        <div class="container">

            <div class='demo' v-for="url in videoData" :key="url">
                <div class="card">
                    <router-link :to="{ path: '/teachervideo', query: { videoTotalId: url.id } }">
                        <el-image  class="cover" :src="$store.state.baseApi + url.coverUrl" style="height: 120px"></el-image>
                    </router-link>
                    <h3>{{ url.topic }}</h3>
                    <el-button type="danger" @click="deleteAllVideos(url.id)"> 删除教学视频集</el-button>
                </div>
            </div>
        </div>
        <div style="height: 20px"></div>

        
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]" :page-size="page.pageSize" layout="total, sizes, prev, pager, next, jumper"
            :total="total" style="text-align: center;margin-top:30px">
        </el-pagination>
    </div>
</template>

<script>
import { listVideo, savaVideo, deleteVideos, pageVideo } from '../../api/video.js'
import Cookies from "js-cookie";
export default {
    name: "Video",
    data() {
        return {
            goDealDialogVisible: false,
            page: {
                page: 1, //初始页
                pageSize: 10,    //    每页的数据
                userId: '',
                roleId: '',
            },

            uploadVideo: {
                topic: '',
                userId: 0,
            },


            videoData: [],
            total: 0,
            videoParam: {
                fileList: '',
                title: '',
                userId: '',
            },
            deleteparam: {
                id: ''
            }

        }
    },
    created() {
        this.page.userId = Cookies.get('userId')
        this.page.roleId = Cookies.get('roleId')
        this.videoParam.userId = Cookies.get('userId')
        this.listAllVideos(this.page)

        console.log(this.$store.state.baseApi + this.url.coverUrl)
    },
    methods: {
        goDealHandleClose() {

        },
        deleteAllVideos(id) {
            this.deleteparam.id = id
            this.$confirm('此操作将永久删除该视频集下的所有视频, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                deleteVideos(this.deleteparam).then(resp => {
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    });
                    this.listAllVideos(this.page)
                })
                this.listAllVideos(this.page)
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                });
            });



        },


        uploadddVideo() {
            this.goDealDialogVisible = true
            this.uploadVideo.userId = Cookies.get('userId')
            this.listAllVideos(this.page)
        },


        submit(videoParam) {
            this.$refs.upload.submit();
            this.goDealDialogVisible = false
            this.listAllVideos(this.page)
            // savaVideo(videoParam).then(resp=>{
            //     if(resp.data.code==200){
            //         this.$message({
            //             message: '新增视频集成功',
            //             type: 'success'
            //         });
            //         this.listAllVideos(this.page)
            //     }else{
            //         this.$message.error('新增视频集失败');
            //     }
            // })
        },
        listAllVideos(page) {
            pageVideo(page).then(resp => {
                this.videoData = resp.data.resultData.records
                this.total = resp.data.resultData.total
            })
        },
        handleSizeChange(size) {
            this.page.pageSize = size;

        },
        handleCurrentChange(pageNum) {
            console.log(pageNum)
            this.page.page = pageNum;
            this.listAllVideos(this.page)
        },

        handleRemove(file, fileList) {
            console.log(file, fileList);
        },
        handlePreview(file) {
            console.log(file);
        },
        handleExceed(files, fileList) {
            this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
        },
        beforeRemove(file, fileList) {
            return this.$confirm(`确定移除 ${file.name}？`);
        }

    }
}
</script>

<style scoped>
.demo {
    /*height: 20%;*/
    text-align: center;
    line-height: 20%;
    border: 1px solid transparent;
    border-radius: 4px;
    overflow: hidden;
    background: #fff;
    position: relative;
    box-shadow: 0 1px 1px rgba(0, 0, 0);
    margin-right: 4px;
    flex-wrap: wrap;
    margin-bottom: 20px;

    position: relative;
    display: flex; /* flex容器（flex container） */
    justify-content: center; /* 定义项目在主轴上的对齐方式 */
    align-items: center; /* 定义项目在竖直方向上对齐方式 */
    max-width: 1200px;
    flex-wrap: wrap; /* 定义项目是否换行以及如何换行 */
    z-index: 1;
}

.container{
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    margin-left: 90px;
    width: 100%;
}
 .container .demo{
     position: relative;
     width: 250px;
     height: 250px;
     margin: 30px;
     /* 设置阴影 */
     box-shadow: 20px 20px 50px rgba(0, 0, 0, 0.5);
     border-radius: 15px;
     /* 设置card的背景色 */
     background: rgba(255, 255, 255, 0.1);
     overflow: hidden;
     display: flex;
     justify-content: center;
     align-items: center;
     border-top: 1px solid rgba(255, 255, 255, 0.5);
     border-left: 1px solid rgba(255, 255, 255, 0.5);
     backdrop-filter: blur(5px);
 }
 .card{
    transition: 0.5s;
}

.card:hover .cover{
  /* perspective(500px) 设置平面和用户眼睛之间的距离*/
  /* rotateX(25deg) 鼠标移入card元素时将其中的cover元素绕x轴旋转25°*/
  transform: perspective(500px) rotateX(25deg);
  /* 设置一个下拉阴影，并设置阴影的颜色*/
  box-shadow: 0 35px 0px 10px rgba(0, 0, 0,0.7);
}


el-button {
    display: inline-block;
}

.course {
    display: inline-block;
}

.ta {
    display: flex;
    justify-content: space-between;
}

.dem {
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
}

.el-image__inner {
    vertical-align: top;
    height: 200px;
}

.demo:hover {
    display: block;
}
</style>