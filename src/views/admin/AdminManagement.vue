<template>
    <div>
        <el-container>
            <el-header>
                <Header></Header>
            </el-header>
            <el-container>
                <div class="kuang">
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                    <div class="bubble"></div>
                </div>
                <el-aside width="200px">
                    <Aside></Aside>
                </el-aside>
                <el-main>
                    <div class="welcome">
                        欢迎访问管理员端
                    </div>
                    
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
import Header from "./header/Header.vue"
import Aside from "./aside/Aside";
export default {
    name: "AdminManagement",
    components: { Aside, Header }
}
</script>

<style scoped>
.el-container.is-vertical {
    flex-direction: column;
    height: 100%;
}

.el-header[data-v-9aae6930],
.el-footer[data-v-9aae6930] {
    margin-left: -20px;
    margin-right: -20px;
    background-color: #020202;
    color: #333;
    text-align: center;
    line-height: 60px;

    border-bottom: 1px solid #ffffff;
}

.el-aside {
    background-color: #000000;
    color: #333;
    text-align: center;
    line-height: 200px;
}

.aside[data-v-2c9fb0d2] {
    margin-top: 10px;
    background-color: #14252f;
}

.welcome {
    font-size: 34px;
    font-weight: 600;
}

.el-main {
    color: #fffcfc;
    overflow-y: auto;
    height: 90vh;
}

body>.el-container {
    margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
    line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
    line-height: 320px;
}



.kuang{
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -10;
            background-image: linear-gradient(180deg,rgb(0, 0, 0),rgb(0, 0, 0));

        }
        .bubble{
            position: absolute;
            border-radius: 50%;
            border: 2px solid #fff;
            box-shadow: inset 0 0 8px  #fff;
            animation: flutter 10s infinite;
            opacity: 0;
        }
        @keyframes flutter {
            0%{
                transform: translateX(0);
                bottom: -100px;
                opacity: 1;
            }
            50%{
                transform: translateX(100px);
                opacity: 0.5;
            }

            
            100%{
                transform: translateX(0px);
                bottom: 100%;
                opacity: 0;

            }
        }
        .bubble:nth-child(1){
            left: -10%;
            width: 50px;
            height: 50px; 
            animation-duration: 9s;
            animation-delay: 0.1s;
        }
        .bubble:nth-child(2){
            left: 15%;
            width: 20px;
            height: 20px;
            animation-duration: 6s;
            animation-delay: 1.5s;

        }
        .bubble:nth-child(3){
            left: 20%;
            width: 60px;
            height: 60px;
            animation-duration: 10s;
            


        }
        .bubble:nth-child(4){
            left: 30%;
            width: 30px;
            height: 30px;
            animation-duration: 5.5s;
            animation-delay: 1.5s;

        }
        .bubble:nth-child(5){
            left: 40%x;
            width: 50px;
            height: 50px;
            animation-duration: 12s;
            


        }
        .bubble:nth-child(6){
            left: 50%;
            width: 20px;
            height: 20px;
            animation-duration: 6s;
            animation-delay: 1s;

        }
        .bubble:nth-child(7){
            left: 60%;
            width: 40px;
            height: 40px;
            animation-duration: 8s;
            animation-delay: 1s;

        }
        .bubble:nth-child(8){
            left: 65%;
            width: 60px;
            height: 60px;
            animation-duration: 15s;
          
        }
        .bubble:nth-child(9){
            left: 80%;
            width: 55px;
            height: 55px;
            animation-duration: 9s;
            animation-delay: 0.5s;
            
        }
        .bubble:nth-child(10){
            left: 100%;
            width: 40px;
            height: 40px;
            animation-duration: 12s;

        }

</style>
