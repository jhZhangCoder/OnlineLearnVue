<template>
    <div>
        <!-- <el-row :gutter="24" style="height: 80px">
            <el-col :span="10" style="text-align: right;">
                <img src="@/assets/03.png" style="
                    width: 40px;
                    height: 40px;
                    padding: 15px 0 0 0;
                    -webkit-user-drag: none;
                    -khtml-user-drag: none;
                    -moz-user-drag: none;
                    user-drag: none;
                " />
            </el-col>
            <el-col :span="14" style="font-size: 32px;font-weight: 600;text-align: left;margin: 15px 0 0 0;">
                个人学习助手
            </el-col>
        </el-row> -->
        

        <div style="margin: 0 -20px 0 -20px;padding: 0;">
            <el-menu :background-color="'black'" :text-color="'white'" :active-text-color="'white'" :default-active="'1'"
                class="el-menu-demo" :router="true" mode="horizontal" @select="handleSelect">
                <el-menu-item index="/home">
                    <i class="iconfont icon-r-home" style="color: white;font-size: 22px;"></i>
                    首页
                </el-menu-item>
                <el-submenu index="1" style="font-size: 20px">
                    <template slot="title">
                        <span style="font-size: 20px;">
                            <i class="iconfont icon-r-paper" style="color: white;font-size: 22px;"></i>
                             作业管理
                        </span>
                    </template>
                    <el-submenu index="1-1">
                        <template slot="title"><span style="font-size: 20px;">
                    <i class="iconfont icon-r-find" style="color: white;font-size: 22px;"></i> 查看作业</span> </template>
                        <el-menu-item index="/checkhomework">
                    <i class="iconfont icon-r-yes" style="color: white;font-size: 22px;"></i> 已完成</el-menu-item>
                        <el-menu-item index="/nohomework">
                    <i class="iconfont icon-r-refresh" style="color: white;font-size: 22px;"></i> 未完成</el-menu-item>
                    </el-submenu>
                    <el-menu-item index="/askandanswer">
                    <i class="iconfont icon-r-shield" style="color: white;font-size: 22px;"></i> 问答</el-menu-item>
                    <el-menu-item index="/studentpractice">
                    <i class="iconfont icon-r-edit" style="color: white;font-size: 22px;"></i> 试题</el-menu-item>

                </el-submenu>
                <el-menu-item index="/onlinelearn">
                    <i class="iconfont icon-r-team" style="color: white;font-size: 26px;"></i> 在线学习</el-menu-item>
                <el-menu-item index="/studentmanagement">
                    <i class="iconfont icon-r-building" style="color: white;font-size: 22px;"></i> 班级列表</el-menu-item>
                <el-menu-item index="/practicedetail">
                    <i class="iconfont icon-r-find" style="color: white;font-size: 22px;"></i> 习题提交记录</el-menu-item>
                <el-menu-item index="/essentiainfo">
                    <i class="iconfont icon-r-user2" style="color: white;font-size: 22px;"></i> 基本信息</el-menu-item>
                <div class="cn">
                    <div class="blockl">

                        <el-submenu index="2">
                            <template slot="title">
                                <div class="demo-fit">
                                    <div class="block">
                                        <i style="font-size: 50px;color: #ffffff; margin-right: 20px;"
                                            class="iconfont icon-r-user2"></i>
                                    </div>
                                </div>
                            </template>
                            <el-menu-item index="/checkhomework" @click="logout()">
                    <i class="iconfont icon-r-left" style="color: white;font-size: 22px;"></i> 退出</el-menu-item>
                            <el-menu-item @click="change()">
                    <i class="iconfont icon-r-lock" style="color: white;font-size: 22px;"></i> 修改密码</el-menu-item>
                        </el-submenu>
                    </div>
                </div>
            </el-menu>

        </div>


            <el-dialog  :modal-append-to-body='false' title="修改密码" :visible.sync="dialogFormVisible" width="30%" :before-close="handleClose">

                <el-input placeholder="请输入原密码"  v-model="changePassword.password" show-password></el-input>
                <p>

                    <el-input placeholder="请输入新密码" v-model="changePassword.newPassword" show-password></el-input>
                </p>
                <span slot="footer" class="dialog-footer">
                    <el-button @click="dialogFormVisible = false"> 取 消</el-button>
                    <el-button type="primary" @click="submit(changePassword)" style="background-color: gray;"> 确 定</el-button>
                </span>
            </el-dialog>



    </div>
</template>

<script>
import Cookies from "js-cookie";
import { password } from '../../../../api/personal.js'

export default {
    name: "Header",
    data() {
        return {
            changePassword: {
                password: '',
                newPassword: '',
                id: ''
            },
            dialogFormVisible: false,
            info: {
                password: '',
                newPassword: '',
                id: ''
            },
            drawer: false,
            url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
        }
    },
    created() {
        this.changePassword.id = Cookies.get("userId")
    },
    methods: {

        handleSelect() {

        },
        handleClose(done) {
            this.$confirm('确认关闭？')
                .then(_ => {
                    done();
                })
                .catch(_ => { });
        },
        change() {
            this.dialogFormVisible = true
        },

        submit(da) {
            password(da).then(resp => {
                if (resp.data.code == 200) {
                    this.$message({
                        message: '密码修改成功 ',
                        type: 'success'
                    });
                    this.dialogFormVisible = false
                    this.studentquery(this.page)
                } else {
                    this.$message.error('原密码错误');
                }
            })
        },
        logout() {
            Cookies.remove('userId')
            Cookies.remove('classId')
            Cookies.remove('roleId')
            this.$router.push('/login')
            this.$message({
                message: '退出成功',
                type: 'success'
            });
        }
    }
}
</script>

<style scoped>
.el-menu-item {
    font-size: 20px
}



h1 {
    /* position: absolute; */
    margin-left: 40%;
}

/* .el-menu--horizontal>.el-submenu .el-submenu__title {
    height: 60px;
    line-height: 60px;
    border-bottom: 2px solid transparent;
    color: #ffffff !important;
} */

.el-dialog__wrapper {
    z-index: 9999 !important;
}

.blockl {
    position: absolute;
    right: 0px;
}

.el-menu {
    border-right: solid 1px #e6e6e6;
    list-style: none;
    /* position: relative; */
    margin: 0;
    padding-left: 0;
    
}

.el-menu--horizontal>.el-menu-item[data-v-15228138] {
    /* float: left; */
    height: 60px;
    line-height: 60px;
    margin: 0;
    border-bottom: 2px solid transparent;
    color: #ffffff;
   
}

/* .el-menu--horizontal>.el-submenu .el-submenu__title {
    height: 60px;
    line-height: 60px;
    border-bottom: 2px solid transparent;
    color: #ffffff;
} */


.cn {
    display: flex;
    justify-content: space-between;
}   

.el-button {
    width: 48%;
    animation: tada 1s;
}
.el-button:active{
  animation: none;
}

@keyframes tada {
    from {
        transform: scale3d(1, 1, 1)
    }
    10%, 20% {
        transform: scale3d(.9, .9, .9) rotate3d(0, 0, 1, -3deg)
    }
    30%, 50%, 70%, 90% {
        transform: scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, 3deg)
    }
    40%, 60%, 80% {
        transform: scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, -3deg)
    }
    to {
        transform: scale3d(1, 1, 1)
    }
}
</style>