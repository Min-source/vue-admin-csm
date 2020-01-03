<template>
    <div>
        <h3>产品管理</h3>
        <el-button size="small" type="primary">添加</el-button>
        <el-button size="small" type="danger">删除</el-button>
        <el-table :data="products">
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="产品名称" prop="name"></el-table-column>
            <el-table-column label="价格" prop="price"></el-table-column>
            <el-table-column label="描述" prop="description"></el-table-column>
            <el-table-column label="所属产品" prop="categoryId"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toHandler(slot.row.id)">详情</a>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
    data(){
        return{
            products:[]
        }
    },
    created(){
        this.loadData()
    },
    methods:{
        loadData(){
            let url="http://localhost:6677/product/findAll"
            requert.get(url).then((response)=>{
                this.products=response.data;
            })
        },
        toDeleteHandler(){
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
        toUpdateHandler(){

        }
    }
}
</script>
<style scoped>

</style>