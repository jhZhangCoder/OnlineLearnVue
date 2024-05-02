<template>
    <div style="background-color: rgb(240, 248, 255, 0.5);">
        <div class="ti"><b>问答社区</b></div>
        <el-divider></el-divider>
        <div v-for="t in NotHomeWork" :key="t.id" class="content">
            <h4 class="colortxt">问题来源：{{ t.topic }}</h4>
            <span class="light"></span>
            <p style="color: rgb(16, 15, 15); font-size: 24px">
                <i class="iconfont icon-r-shield" style="font-size: 26px"></i>
                <b class="threesolid">{{ t.senderName }} 问题描述：{{ t.content }}</b>
            </p>
            <b class="reback">{{ t.recipientName }} 问题回复：</b>

            <p class="reback"  v-if="t.restore != undefined">
                <i class="iconfont icon-r-yes" ></i>
                <b>{{ t.restore }}</b>
            </p>
            <p class="reback" style="color: rgb(249, 247, 247);" v-if="t.restore == undefined">
                <i class="iconfont icon-r-refresh" ></i>
                <b>等待回复 ...</b>
            </p>

            <el-divider>
                <span class="status">{{ t.status }}</span>
            </el-divider>
        </div>
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]"
            :page-size="page.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="NotHomeWork.length"
            style="text-align: center; margin-top: 20px"
        ></el-pagination>
    </div>
</template>

<script>
import { askandanswer } from '../../../api/studentweb/askandanswer.js';
import Cookies from 'js-cookie';
export default {
    name: 'AskAndAnswer',
    data() {
        return {
            page: {
                page: 1, //初始页
                pageSize: 10, //    每页的数据
                userId: 0,
            },
            NotHomeWork: [],
        };
    },
    created() {
        this.page.userId = Cookies.get('userId');
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
            askandanswer(valid).then(resp => {
                this.NotHomeWork = resp.data.resultData.data;
            });
        },
    },
};
</script>

<style scoped>
h4 {
    display: flex;
    justify-content: center;
}

.right {
    display: flex;
    justify-content: flex-end;
}

.ti {
    font-size: 28px;
    width: 100%;
    text-align: center;
    display: flex;
    justify-content: center;
    font-family: 'Microsoft YaHei';
    font-size: 28px;
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
.status {
    font-size: 20px;
}

.colortxt {
    font-size: 30px;
    background: linear-gradient(to right, rgb(26, 26, 26), rgb(65, 65, 66));
    -webkit-background-clip: text;
    color: transparent;
    font-weight: 500; /*设置字体粗细*/
    text-align: center; /*设置文字居中*/
    -webkit-animation: bounce 2s infinite; /*设置动画*/
}
@-webkit-keyframes bounce {
    /*创建动画*/
    0%,
    100%,
    20%,
    50%,
    80% {
        -webkit-transform: translateY(0);
    }
    40% {
        -webkit-transform: translateY(-30px);
    }
    60% {
        -webkit-transform: translateY(-15px);
    }
}

.light {
    font-size: 30px;
    color: #080808;
    text-shadow: 0 0 0.5em #1e1d1d, 0 0 0.2em #1f1f1f;
}

.threesolid {
    font-size: 20px;
    color: #0b0b0b;
    text-shadow: 0px 1px 0px #c0c0c0, 0px 2px 0px #f6f4f4, 0px 3px 0px #e8e7e7, 0px 4px 0px #dbd9d9,
        0px 5px 10px rgba(122, 122, 122, 0.9);
}

.content {
    margin-left: 40px;
}

.jump {
    font: 96px '微软雅黑'; /*设置字体和字体大小*/
    margin: 50px auto; /*设置元素外边距*/
    font-weight: 500; /*设置字体粗细*/
    text-align: center; /*设置文字居中*/
    color: #f35626; /*设置文字颜色*/
    -webkit-animation: bounce 2s infinite; /*设置动画*/
}

.reback{
    font-family:"微软雅黑";   /*设置字体*/
		   font-size: 20px;   /*设置文字大小*/
		   color:solid;  /*设置文字颜色透明*/
		   text-shadow: 0 0px 6px #111111;  /*设置文字阴影*/

}
</style>
