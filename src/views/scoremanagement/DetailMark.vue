<template>
    <div>
        <router-link to="/scoreManagment">
            <el-button type="success"> 返回</el-button>
        </router-link>
        <div style=""> 
            <mavon-editor
                    :subfield="false"
                    :autofocus="false"
                    v-model=$route.params.data1
                    ref="md"
                    style="width: 99%;height: 90vh;"
            >
            </mavon-editor>
        </div>
    </div>

</template>


<script>
    export default {
        name: "DetailMark"
    }
</script>

<style scoped>

</style>