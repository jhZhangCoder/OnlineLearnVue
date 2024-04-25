<template>
    <div>
        <div class="dem">

            <div class='demo' v-for="url in iData" :key="url.id">
                <div>
                    <router-link :to="{ path: '/detailonlineweb', query: { videoTotalId: url.id, userId: url.userId } }">
                        <el-image style="height: 120px" :src="$store.state.baseApi + url.coverUrl"></el-image>
                    </router-link>
                    <h3>{{ url.topic }}</h3>
                    <div class="ta">
                        {{ url.userName }}
                        <!-- <el-tag> {{url.createTime}}</el-tag> -->
                    </div>


                </div>

            </div>

        </div>
        <div style="height: 40px"></div>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]" :page-size="page.pageSize" layout="total, sizes, prev, pager, next, jumper"
            :total="total">
        </el-pagination>
    </div>
</template>
<script>
import { onlineweb, onlinecourse } from '../../../api/studentweb/online.js'
import { videos } from "../../../api/studentweb/video.js";
import Cookies from "js-cookie";
export default {
    name: "OnLineLearn",
    data() {
        return {
            page: {
                page: 1, //初始页
                pageSize: 10,    //    每页的数据
                classId: ''
            },
            iData: [],
            videos: [],
            videoss: {
                videoTotalId: ''
            },
            course: {
                classId: ''
            },
            onLineCou: [],

            total: 0

        }
    },
    created() {
        this.page.classId = Cookies.get('classId')
        this.course.classId = Cookies.get('classId')
        this.listAllCourse(this.course)
        this.listAllStudentsScore(this.page)
    },
    methods: {


        detailvideo(video) {
            this.videoss.videoTotalId = video
            videos(this.videos).then(resp => {
                this.videos = resp.data.resultData.data

            })
        },
        handleSizeChange(size) {
            this.page.pageSize = size;
            this.listAllStudentsScore(this.page)
            // console.log(this.pageSize,'888')

            console.log(`每页 ${size} 条`);
        },
        handleCurrentChange(pageNum) {
            this.page.page = pageNum;
            this.listAllStudentsScore(this.page)
            console.log(`当前页: ${pageNum}`);
        },

        listAllCourse(page) {
            onlinecourse(page).then(resp => {
                this.onLineCou = resp.data.resultData
            })
        },
        listAllStudentsScore(page) {
            onlineweb(page).then(resp => {
                console.log(resp)
                this.iData = resp.data.resultData.records
                this.total = resp.data.resultData.total


            })
        }
    }

}
</script>
<style scoped>
.el-image__inner {
    vertical-align: top;
    width: 200px;
    height: 100px;
}

.demo {
    width: 255px;
    text-align: center;
    line-height: 15%;
    border: 1px solid transparent;
    border-radius: 4px;
    overflow: hidden;
    background: #fff;
    position: relative;
    box-shadow: 0 1px 1px rgba(0, 0, 0, .2);
    margin-right: 4px;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-top: 20px;

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
    height: 30px;
    line-height: 30px;
}

.dem {
    display: flex;
    justify-content: flex-start;

    flex-wrap: wrap;
}

</style>