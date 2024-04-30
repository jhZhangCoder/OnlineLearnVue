<template>
    <div>
    <el-carousel :interval="4000" type="card" height="400px">
        <el-carousel-item >
            <img src="@/assets/st-bg.jpg" alt="some_text" style="height: 100%">
        </el-carousel-item>
        <el-carousel-item >
            <img src="@/assets/tc-bg.jpg" alt="some_text" style="height: 100%">
        </el-carousel-item>
        <el-carousel-item >
            <img src="@/assets/ad-bg.jpg" alt="some_text" style="height: 100%">
        </el-carousel-item>
    </el-carousel>
    <div style="margin-bottom: 20px;margin-top: 80px"> 
        <div id="tips"style="background-color: rgba(248, 247, 247, 0.5);border: 0;">
            
            <b>
                补充知识点
            </b>
        </div> 
        <el-card v-for="item2 in points " class="box-card" :key="item2.id" style="background-color: rgba(248, 247, 247, 0.5);border: 0;">
            <div slot="header" class="clearfix">
                <span> {{ item2.title }}</span> 
            </div>
            <div class="text item">
                {{ item2.content }} 
            </div>
        </el-card>
    </div>
</div>
</template>

<script>
import {stuPointList} from '@/api/admin/knowpoint'
import Cookies from "js-cookie";
export default {
    name: "home",
    data() {
        return {
            points:[]
        }
    },
    created() {
        let classId=Cookies.get("classId")
        stuPointList({classId:classId}).then(res=>{
            this.points=res.data.resultData
            console.log(this.points);    
        })

     }
}
</script>

<style scoped>
.con{
    height: 100vh!important;
    overflow-y: auto!important;
}

#tips {
    font-size: 26px;
    font-weight: 600;
}
</style>