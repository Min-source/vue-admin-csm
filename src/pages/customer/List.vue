<template>
    <div>
        <!--按钮-->
        <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>
        <!--/按钮-->
        <!--表格-->
        <el-table :data="customers">
        <el-table-column label="编号" prop="id"></el-table-column>
        <el-table-column label="姓名" prop="realname"></el-table-column>
        <el-table-column label="手机号" prop="telephone"></el-table-column>
        <el-table-column label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row.id)">修改</a>
            </template>
        </el-table-column>
        </el-table>
        <!--/表格-->
        <!--分页-->
        <div><el-pagination small
           layout="prev, pager, next"
           :total="50">
        </el-pagination>
        </div>
        <!--/分页-->
        <!--对话框-->
        <el-dialog
            :title="title"
            :visible.sync="visible"
            width="60%">
            {{form}}
                <el-form :model="form" label-width="80px">
                    <el-form-item label="用户名">
                        <el-input v-model="form.username"></el-input>
                    </el-form-item>
                    <el-form-item label="密码">
                        <el-input type="password" v-model="form.password"></el-input>
                    </el-form-item>
                    <el-form-item label="真实姓名">
                        <el-input type="realname" v-model="form.realname"></el-input>
                    </el-form-item>
                    <el-form-item label="手机号">
                        <el-input type="telephone" v-model="form.telephone"></el-input>
                    </el-form-item>
                </el-form>

            <span slot="footer" class="dialog-footer">
            <el-button size="small" @click="closerModalHandler">取 消</el-button>
            <el-button type="primary" size="small" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
        <!--/对话框-->
    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from "querystring"//将查询对象转换成字符串/流
export default {
        //用于存放要想网页显示的数据
    data(){
        return{
            visible:false,//数据状态
            customers:[],
            form:{
                type:"customer"
            }
        }
    },
    created(){
       //vue创建实例完毕
        this.loadData()
   },
    //用于存放网页中需要调用的方法
    methods:{
        loadData(){
            //vue创建实例完毕
        let url="http://localhost:6677/customer/findAll"
        request.get(url).then((response)=>{
          //this为当前vue实例对象
          this.customers=response.data;
        })
        },
        toAddHandler(){
            this.form={
                type:"customer"
            };
            this.title="录入顾客信息";
            this.visible=true;
        },
        closerModalHandler(){
            this.visible=false;
        },
        toUpdateHandler(row){
            this.title="修改顾客信息";
            this.form=row;//模态框显示当前行
            this.visible=true;
        },
        toDeleteHandler(id){
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            //调用后台接口，完成删除操作
            let url="http://localhost:6677/product/deleteById?id="+id;
            request.get(url).then((response)=>{
              //1.刷新数据
              this.loadData();
              //2.提示结果
              this.$message({
              type: 'success',
              message:response.message
            });
            })
        })
        },
        submitHandler(){
            //前端this.form对象转换成字符串，后台将字符串转换成Java对象，存到数据库里
            //通过request与后台交互，并且携带参数
            let url="http://localhost:6677/customer/saveOrUpdate";
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
            //post封装 request.post(url,this.form)
        }
    }
}
</script>

<style scoped>

</style>