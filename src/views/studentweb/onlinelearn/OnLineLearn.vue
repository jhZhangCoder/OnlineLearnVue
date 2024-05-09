<template>
    <div class="total">
        <div class="container">
            <div class="demo" v-for="url in iData" :key="url.id">
                <div class="card">
                    <el-image
                            style="height: 120px;margin-top: 10px;"
                            :src="$store.state.baseApi + url.coverUrl"
                            class="cover"
                        ></el-image>
                    
                    <h3 style="color: white;">{{ url.topic }}</h3>
                    <router-link
                        :to="{
                            path: '/detailonlineweb',
                            query: { videoTotalId: url.id, userId: url.userId },
                    }">
                        <el-button type="info">查看详情</el-button>
                    </router-link>

                </div>
            </div>
        </div>
        <div style="height: 40px"></div>
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]"
            :page-size="page.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
            style="text-align: center;margin-top:30px"
        ></el-pagination>
    </div>
</template>
<script>
import { onlineweb, onlinecourse } from '../../../api/studentweb/online.js';
import { videos } from '../../../api/studentweb/video.js';
import Cookies from 'js-cookie';
export default {
    name: 'OnLineLearn',
    data() {
        return {
            page: {
                page: 1, //初始页
                pageSize: 10, //    每页的数据
                classId: '',
            },
            iData: [],
            videos: [],
            videoss: {
                videoTotalId: '',
            },
            course: {
                classId: '',
            },
            onLineCou: [],

            total: 0,
        };
    },
    created() {
        this.page.classId = Cookies.get('classId');
        this.course.classId = Cookies.get('classId');
        this.listAllCourse(this.course);
        this.listAllStudentsScore(this.page);
    },
    methods: {
        detailvideo(video) {
            this.videoss.videoTotalId = video;
            videos(this.videos).then(resp => {
                this.videos = resp.data.resultData.data;
            });
        },
        handleSizeChange(size) {
            this.page.pageSize = size;
            this.listAllStudentsScore(this.page);
            // console.log(this.pageSize,'888')

            console.log(`每页 ${size} 条`);
        },
        handleCurrentChange(pageNum) {
            this.page.page = pageNum;
            this.listAllStudentsScore(this.page);
            console.log(`当前页: ${pageNum}`);
        },

        listAllCourse(page) {
            onlinecourse(page).then(resp => {
                this.onLineCou = resp.data.resultData;
            });
        },
        listAllStudentsScore(page) {
            onlineweb(page).then(resp => {
                console.log(resp);
                this.iData = resp.data.resultData.records;
                this.total = resp.data.resultData.total;
            });
        },
    },
};
</script>
<style scoped>
.el-image__inner {
    vertical-align: top;
    width: 200px;
    height: 100px;
}

.demo {
    width: 255px;
    height: 300px;
    text-align: center;
    line-height: 15%;
    border: 1px solid transparent;
    border-radius: 4px;
    overflow: hidden;
    background: #fff;
    position: relative;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2);
    margin-right: 4px;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-top: 20px;

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


</style>
