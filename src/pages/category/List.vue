<template>
    <div> 
    <el-button type="success" size="small" @click="toAddHandler" >添加</el-button>
   <el-button type="danger" size="small" >批量删除</el-button>
   <el-table :data="categorys">
    <el-table-column prop="id" label="编号"></el-table-column>
    <el-table-column prop="name" label="栏目名称"></el-table-column>
    <el-table-column prop="num" label="序号"></el-table-column>
    <el-table-column prop="parentId" label="父栏目"></el-table-column>
    <el-table-column label="操作">
    <template v-slot="slot">
<a href="" @click.prevent="toDeleteHandle(slot.row.id)">删除</a>
<a href="" @click.prevent="toUPdataHandle">修改</a>
    </template></el-table-column>
   </el-table>
  <!-- 表格结束 -->
<!-- 分页开始 -->
   <el-pagination
    layout="prev, pager, next"
    :total="50">
  </el-pagination>  
  <!-- 分页结束 -->  
  <!-- 对话框 -->  
  <el-dialog  
  title="录入栏目信息"  
  :visible.sync="visible"  
  width="60%"  
  >  
  <el-form :model="form" label-width="80px"  
  >
  <el-form-item label="栏目名称">
  <el-input  v-model="form.name">
    </el-input>
    </el-form-item>
  <el-form-item label="序号">
  <el-input  v-model="form.num">
    </el-input>
    </el-form-item>
  
     
  </el-form>
  <span slot="footer" class="dialog-footer">  
    <el-button size="small" @click="closeModalhandle">取 消</el-button>   
    <el-button  size="small" type="primary" @click="submitHandler">确 定</el-button>     
  </span>
</el-dialog>
    </div>

    
</template>
<script>
import request from'@/utils/request'
import querystring from 'querystring'
export default {
//用于存取网页中需要调用的方法
methods:{
  loaddata(){

     let url ="http://localhost:6677/category/findAll"
      request.get(url).then((response)=>{
        this.categorys =response.data;
        //将查询结果设置到customers，this指向外面的函数
      })
  },
submitHandler(){
  //this.form 对象--  字符串-->后台
//通过REQUEST与后台交互，并且要携带参数
let url ="http://localhost:6677/category/saveOrUpdate"
request({
url,
method:"POST",
handers:{
"Content-Type":"application/x-www-form-urlencoded"

},  
 data:querystring.stringify(this.form)
}).then((response)=>{

//请求结束,关闭模态框
this.closeModalhandle();
this.$message({
//提示成功消息
type:"success",
message:response.message

});
this.loaddata();
})
},
toDeleteHandle(id){
//确认
  this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })
    },
toUPdataHandle(){
this.visible=true
    },
closeModalhandle(){
this.visible=false
},
toAddHandler(){
this.visible=true;
}
},
    //要向网页中显示的数据
    data(){
return{
       visible:false,
      categorys:[],
       form:{
         type:"category"
       }
}
    },
    created() {
      //this为当球vue实例对象
      //vue实例加载完毕
     this.loaddata();
    }
}//暴漏接口，让其他模块接受
</script>
<style scoped>

</style>