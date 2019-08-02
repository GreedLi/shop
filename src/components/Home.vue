<template>
  <div class="user">
    <!-- 搜索框 -->
    <el-input placeholder="请输入内容" v-model="query" class="input-with-select">
      <el-button slot="append" icon="el-icon-search" @click="search"></el-button>
    </el-input>
    <!-- 新增 -->
    <el-button type="success" plain @click="showAddDialog">添加人员</el-button>
    <!-- 表格 -->
    <el-table :data="userList" style="width: 100%">
      <el-table-column prop="username" label="姓名"  width="180"></el-table-column>
      <el-table-column prop="email" label="邮箱"  width="180"></el-table-column>
      <el-table-column prop="mobile" label="电话"  width="180"></el-table-column>
      <el-table-column prop="mg_state" label="用户状态"  width="180">
        <template slot-scope="scope">
          <el-switch
            @change="changeState(scope.row)"
            v-model="scope.row.mg_state"
            active-color="#13ce66"
            inactive-color="#ff4949">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column label="操作"  width="180">
        <template slot-scope="scope">
          <el-button type="primary" @click="showEditDialog(scope.row)" icon="el-icon-edit" size="mini" plain></el-button>
          <el-button type="danger" @click="delUser(scope.row)" icon="el-icon-delete" size="mini" plain></el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 增加对话框 -->
    <el-dialog title="新增人员" :visible.sync="addDialogVisible" width="40%">
      <el-form ref="addForm" :model="addForm" label-width="80px">
        <el-form-item label="用户名" >
          <el-input v-model="addForm.name" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <el-form-item label="密码" >
          <el-input v-model="addForm.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" >
          <el-input v-model="addForm.email" placeholder="请输入邮箱"></el-input>
        </el-form-item>
        <el-form-item label="手机" >
          <el-input v-model="addForm.phone" placeholder="请输入手机"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addUser">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  data() {
    return {
      query:'',
      currentPage: 1,
      pageSize: 10,
      total:0,
      addDialogVisible: false,
      userList:[],
      rules: {
        username: [
          { required: true, message: '用户名不能为空', trigger: 'blur' },
          { min: 3, max: 9, message: '用户名长度在3-9位', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'blur' },
          { min: 6, max: 12, message: '密码长度在6-12位', trigger: 'blur' }
        ],
        email: [
          { type: 'email', message: '请输入一个合法的邮箱', trigger: 'blur' }
        ],
        mobile: [
          {
            pattern: /^1\d{10}$/,
            message: '请输入一个合法的手机号',
            trigger: 'blur'
          }
        ]
      },
      addForm: {
          name: '',
          password:'',
          email:'',
          phone:'',
      },
    }
  },
  methods: {
   getUserList(){
     this.axios({
       methods:'get',
       url:'users',
       params:{
         query:this.query,
         pagesize:this.pageSize,
         pagenum:this.currentPage,
       }
     }).then(res=>{
      //  console.log(res,'121');
      let { meta:{status} , data:{users} } = res
       if (status === 200) {
        this.userList = users
       }
     })
   },
   search(){

   },
   changeState(){

   },
   showAddDialog(){
     this.addDialogVisible = true;
   },
   addUser(){
     this.addDialogVisible = false;
   },
   showEditDialog(){

   },
   delUser(){}
  },
  created() {
    this.getUserList()
  }
}
</script>

<style scoped>
.input-with-select {
  width: 300px;
  margin-bottom: 5px;
}
</style>
