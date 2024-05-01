<template>
    <div style="background-color: rgb(240, 248, 255, 0.5)">
        <div class="subject">
            <h1 class="dohomework">未完成作业详情</h1>
        </div>
        <el-divider></el-divider>
        <div v-for="NH in NotHomeWork" :key="NH.id" class="item">
            <div style="border: 2px; ">
                <h2 style="margin-left: 40px;">学科：{{ NH.subjectName }}</h2>
                <span style="margin-left: 40px;">学生：{{ NH.userName }}</span>
                <P style="margin-left: 40px;">作业标题：{{ NH.title }}</P>

                <!--            <router-link to="/markdownnothomework">-->
                <div class="bt">
                    <el-button type="info"style="background-color:black"  @click="addNotHomework(NH)">
                        开始答题
                    </el-button>
                    <!--                    <el-button type="primary" plain  @click="addNotHomework(NH.answer)"> 查看参考答案</el-button>-->
                </div>
                <!--            </router-link>-->
                <h2 class="bt" style="color: white; font-size: 15px;margin-right: 80px;">提交时间：{{ NH.commitTime }}</h2>
                <el-divider class="div1"></el-divider>
            </div>
        </div>
        <el-divider></el-divider>
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]"
            :page-size="page.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="NotHomeWork.length"
            style="text-align: center;"
        ></el-pagination>
    </div>
</template>

<script>
import { work } from '../../../../api/studentweb/dohomework.js';
import Cookies from 'js-cookie';
export default {
    name: 'NoHomeWork',
    data() {
        return {
            page: {
                page: 1, //初始页
                pageSize: 10, //    每页的数据
                userId: 0,
                roleId: 0,
                classId: 0,
            },
            NotHomeWork: [],
        };
    },
    created() {
        this.page.userId = Cookies.get('userId');
        this.page.roleId = Cookies.get('roleId');
        this.page.classId = Cookies.get('classId');
        this.CheckWork(this.page);
    },
    methods: {
        handleSizeChange(size) {
            this.page.pageSize = size;
            this.listAllStudentsScore(this.page);
            // console.log(this.pageSize,'888')

            console.log(`每页 ${size} 条`);
        },
        handleCurrentChange(pageNum) {
            this.page.pageNum = pageNum;
            this.listAllStudentsScore(this.page);
            console.log(`当前页: ${pageNum}`);
        },
        CheckWork(valid) {
            work(valid).then(resp => {
                this.NotHomeWork = resp.data.resultData.data;
            });
        },
        addNotHomework(NotHomeWork) {
            this.$router.push({
                name: 'MarkDownNotHomeWork',
                params: {
                    data1: NotHomeWork,
                    data2: '123',
                },
            });
            // this.$store.dispatch('addNotHomework',NotHomeWork)
        },
    },
};
</script>

<style scoped>
.bt {
    display: flex;
    justify-content: flex-end;
    margin-right: 100px;
}

.dohomework {
    display: flex;
    justify-content: center;
    font-family: 'Microsoft YaHei';
    text-align: center;
    font-size: 40px;
    background: -webkit-linear-gradient(left, rgb(245, 245, 245), rgb(19, 19, 19)) 0 0 no-repeat; /*设置线性渐变*/
    -webkit-background-size: 160px; /*设置背景大小*/
    -webkit-background-clip: text; /*背景被裁剪到文字*/
    -webkit-text-fill-color: rgba(0, 0, 0, 0.7); /*设置文字的填充颜色*/
    -webkit-animation: shine 3s infinite; /*设置动画*/
}
@-webkit-keyframes shine {
    /*创建动画*/
    0% {
        background-position: 0 0;
    }
    100% {
        background-position: 100% 100%;
    }
}


</style>
