<template>
    <div style="background-color: rgb(240, 248, 255, 0.5)">
        <div style="margin-right: 20px; text-align: right;">
            <el-button type="info" style="background-color: black;margin-top: 10px;" @click="$router.push('/applicanthistory')">申请记录</el-button>
        </div>
        <el-divider></el-divider>
        <div v-for="s in StudentClassData" :key="s.id">
            <div>
                <span style="margin-left: 40px;">
                    <el-tag type="info">班级： {{ s.className }}</el-tag>
                </span>
                <div style="right: auto;margin-right: 40px;">
                <el-popover
                    placement="top-start"
                    width="100"
                    class="right"
                    trigger="hover"
                    :content="!flag ? '加入该班级' : '无法加入，您当前已经加入班级或加入班级审核中'"
                >
                    <el-button
                        type="info"
                        class="join"
                        :disabled="flag"
                        @click="studentAddClass(s.id, s.userId)"
                        slot="reference"
                    >
                        加入
                    </el-button>
                </el-popover>
            </div>
            </div>
            <el-divider></el-divider>
        </div>
        <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="page.page"
            :page-sizes="[10, 20, 30, 40]"
            :page-size="page.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="StudentClassData.length"
            style="text-align: center;margin-top:30px"
        ></el-pagination>
    </div>
</template>

<script>
import {
    smanamgent,
    addClass,
    detailhistory,
    isFlage,
} from '../../../api/studentweb/studentmanagement.js';
import Cookies from 'js-cookie';

export default {
    name: 'StudentManagement',
    data() {
        return {
            StudentClassData: [],
            page: {
                page: 1, //初始页
                pageSize: 10, //    每页的数据
            },
            addClassStudent: {
                classId: '',
                userId: '',
            },

            applicantHistory: [],
            flag: true,
            param: {
                userId: '',
            },
        };
    },
    created() {
        this.param.userId = Cookies.get('userId');
        this.isAllFlag(this.param);
        this.StudentClass(this.page);
        this.addClassStudent.userId = Cookies.get('userId');
        if (Cookies.get('classId') != 'undefined') {
            console.log(Cookies.get('classId'));
            this.flag = true;
        } else {
            this.flag = false;
        }
    },
    methods: {
        isAllFlag(param) {
            isFlage(param).then(resp => {
                this.flag = resp.data.resultData;
            });
        },
        handleSizeChange(size) {
            this.page.pageSize = size;
            this.StudentClass(this.page);
            // console.log(this.pageSize,'888')
            console.log(`每页 ${size} 条`);
        },
        handleCurrentChange(pageNum) {
            this.page.pageNum = pageNum;
            this.StudentClass(this.page);
            console.log(`当前页: ${pageNum}`);
        },

        StudentClass(page) {
            smanamgent(page).then(resp => {
                this.StudentClassData = resp.data.resultData.data;
                // state.ClassData=resp.data.data.records
                // this.$store.dispatch('classAction',this.ClassData)
            });
        },
        studentAddClass(classId, userId) {
            this.addClassStudent.classId = classId;

            // this.$alert('这是一段内容', '标题名称', {
            //     confirmButtonText: '确定',
            //     callback: action => {
            //         this.$message({
            //             type: 'info',
            //             message: `action: ${ action }`
            //         });
            //     }
            // });

            addClass(this.addClassStudent).then(resp => {
                if (resp.data.code == 200) {
                    this.$message({
                        message: '申请成功，待老师审核',
                        type: 'success',
                    });
                    this.flag = true;
                } else {
                    this.$message.error('不可加入该班级');
                }
            });
        },
    },
};
</script>

<style scoped>
.right {
    display: flex;
    justify-content: right;
}

.join {
    width: 100%;
    animation: tada 1s;
    
}
.join:active {
    animation: none;
}
@keyframes tada {
    from {
        transform: scale3d(1, 1, 1);
    }
    10%,
    20% {
        transform: scale3d(0.9, 0.9, 0.9) rotate3d(0, 0, 1, -3deg);
    }
    30%,
    50%,
    70%,
    90% {
        transform: scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, 3deg);
    }
    40%,
    60%,
    80% {
        transform: scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, -3deg);
    }
    to {
        transform: scale3d(1, 1, 1);
    }
}
</style>
