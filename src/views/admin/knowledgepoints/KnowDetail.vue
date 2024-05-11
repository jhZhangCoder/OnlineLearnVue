<template>
    <div>
        <router-link to="/knowledgepoints">
            <el-button style="margin-left: 200px;margin-bottom: 20px;"> 返回</el-button>
        </router-link>
        <div>
            <mavon-editor
                    disabled="true"
                    :subfield="false"
                    :autofocus="false"
                    v-model=this.$route.params.data1
                    ref="md"
                    style="height: 400px; width: 1000px;margin: 0 auto;"
            >
            </mavon-editor>
        </div>
    </div>
</template>

<script>
    export default {
        name: "KnowDetail"
    }
</script>

<style scoped>

</style>