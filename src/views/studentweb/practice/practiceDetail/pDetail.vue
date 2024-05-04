<template>
    <div class="es">
      <router-link to="/practicedetail"> <el-button > 返回</el-button></router-link>
        <p>试题：</p>
       <p>{{paramData.reply}}</p>
        <p> 参考答案:   </p>
        <div class="div2">
            <mavon-editor
                    :subfield="false"
                    :autofocus="false"
                    v-model="paramData.answer"
                    ref="md"
                    style="height:30%"
                    
            >
            </mavon-editor>
        </div>
    </div>
</template>

<script>
    export default {
        name: "pDetail",
        data(){
            return{
                paramData:{
                    reply:'',
                    answer:'',
                },
            }
        },
        created() {
            this.paramData.reply=this.$route.params.data2.reply
            this.paramData.answer=this.$route.params.data2.answer
        }
    }
</script>

<style scoped>
.es{
    height: 90%;
    width: 85%;
    margin-left: 80px;
    margin-top: 30px;

}
</style>