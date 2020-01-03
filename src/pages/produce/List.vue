<template>
    <div>
        <h3>产品管理</h3>
        <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
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
                    <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
                    <a href="" @click.prevent="toHandler(slot.row.id)">详情</a>
                </template>
            </el-table-column>
        </el-table>
        <el-dialog
            width="60%"
            :title="title"   
            :visible.sync="visible">
            {{form}}
            <el-form :model="form" label-width="80px"> 
                <el-form-item label="名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="价格">
                    <el-input v-model="form.price"></el-input>
                </el-form-item>
                <el-form-item label="所属栏目">
                    <el-input v-model="form.categoryId"></el-input>
                </el-form-item>
                <el-form-item label="介绍">
                    <el-input v-model="form.description"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="closerModalHandler">取 消</el-button>
            <el-button type="primary" size="small" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>
<script>
import request from '@/utils/request'
import querystring from "querystring"
export default {
    data(){
        return{
            visible:false,
            products:[],
            form:{
                type:"produce"
            }
        }
    },
    created(){
        this.loadData()
    },
    methods:{
        loadData(){
            let url="http://localhost:6677/product/findAll"
            request.get(url).then((response)=>{
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
        toUpdateHandler(row){
            this.title="修改产品信息";
            this.visible=true;
            this.form=row;
        },
        closerModalHandler(){
            this.visible=false;
        },
        submitHandler(){
            let url="http://localhost:6677/product/saveOrUpdate";
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"//提示后台传入查询字符串
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
             //模态框关闭
             this.closerModalHandler();
             //刷新 
             this.loadData();
             //提示消息
             this.$message({
                type:"success",
                message:response.message 
             })
            })
        },
        toAddHandler(){
            this.title="录入产品信息";
            this.visible=true;
        }
    }
}
</script>
<style scoped>

</style>