<template>
    <div>
    <h3>栏目管理</h3>
    <el-button size="small" type="primary" @click="toAddDHandler">添加</el-button>
    <el-button size="small" type="danger">删除</el-button>
    <el-table :data="columns">
        <el-table-column label="编号" prop="id"></el-table-column>
        <el-table-column label="栏目名称" prop="name"></el-table-column>
        <el-table-column label="序号" prop="num"></el-table-column>
        <el-table-column label="父栏目" prop="parentId"></el-table-column>
        <el-table-column label="操作" >
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpDateHandler(slot.row.id)">修改</a>
            </template>
        </el-table-column>
    </el-table>
    <el-dialog
            width="60%"
            :title="title"   
            :visible.sync="visible">
            {{form}}
            <el-form :model="form" label-width="80px"> 
                <el-form-item label="栏目名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="序号">
                    <el-input v-model="form.num"></el-input>
                </el-form-item>
                <el-form-item label="父栏目">
                    <el-input v-model="form.parentId"></el-input>
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
            columns:[],
            form:{
                type:"column"
            }
        }
    },
    created(){
       //vue创建实例完毕
        this.loadData()
    },
    methods:{
        loadData(){
            let url="http://localhost:6677/category/findAll"
            request.get(url).then((response)=>{
                this.columns=response.data;
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
        toUpDateHandler(){
            this.title="修改栏目信息";
            this.visible=true;
        },
        toAddDHandler(){
            this.title="录入栏目信息";
            this.visible=true;
        },
        closerModalHandler(){
            this.visible=false;
        },
        submitHandler(){
            let url="http://localhost:6677/category/saveOrUpdate";
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
        }
    }
}
</script>
<style scoped>

</style>