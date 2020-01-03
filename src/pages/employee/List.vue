<template>
   <div>
       <!-- 按钮 -->
       <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
       <el-button size="small" type="danger">批量删除</el-button>
       <!-- 表格 -->
       <el-table :data="employees">
          <el-table-column label="编号" prop="id"></el-table-column>
          <el-table-column label="姓名" prop="realname"></el-table-column>
          <el-table-column label="性别" prop="gender"></el-table-column>
          <el-table-column label="手机号" prop="telephone"></el-table-column>         
          <el-table-column label="操作">
              <template v-slot="slot">
                  <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                  <a href="" @click.prevent="toUpdateHandler(slot.row.id)">修改</a>
              </template>
          </el-table-column>
       </el-table>
       <!-- 分页 -->
       <div><el-pagination small
           layout="prev, pager, next"
           :total="50">
        </el-pagination>
        </div>
       <!-- 模态框 -->
       <el-dialog
            :title="title"
            :visible.sync="visible"
            width="60%">
            {{form}}
            <el-form :model="form" label-width="80px">
                <el-form-item label="用户名">
                    <el-input v-model="form.username"/>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"/>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="form.gender">
                        <el-radio label="男">男</el-radio>
                        <el-radio label="女">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input v-model="form.realname"/>
                </el-form-item>
                <el-form-item label="手机号">
                    <el-input v-model="form.telephone"/>
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
        employees:[],
        form:{
                type:"waiter"
            }
       }
   },
   created(){
      this.loadDate();
   },
   methods:{
       loadDate(){
           //this->vue实例，通过vue实例访问该实例中数据methods，data返回值
           //this.title/this.
           let url="http://localhost:6677/waiter/findAll";
           request.get(url).then((response)=>{
             //this指向外部函数的this
             this.employees=response.data;
           })
       },
       toAddHandler(){
        this.title="录入员工信息";
        this.visible=true;
       },
       closerModalHandler(){
        this.visible=false;
       },
       toDeleteHandler(id){
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
        this.title="修改员工信息";
        this.visible=true;
       },
       submitHandler(){
         let url="http://localhost:6677/waiter/saveOrUpdate";
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
             this.loadDate();
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