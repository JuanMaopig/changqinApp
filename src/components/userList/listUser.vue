<template>
  <div id="app">
    <!--面包屑导航-->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>人员管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb><!--面包屑导航结束-->

    <!--搜索框-->
    <el-row>
      <el-input v-model="input" placeholder="姓名" class="N"></el-input>
      <el-button type="primary" class="C" size="small" @click="searchUser()">查询</el-button>
    </el-row>
    <!--搜索框-->

    <!--表格部分-->
    <el-table
      :data="tableData2"
      style="width: 100%"
      :row-class-name="tableRowClassName">
      <el-table-column
        prop="user_id"
        label="用户编号"
        width="150">
      </el-table-column>
      <el-table-column
        prop="username"
        label="用户名称"
        width="150">
      </el-table-column>
      <el-table-column
        prop="realname"
        label="真实姓名"
        width="150">
      </el-table-column>
      <el-table-column
        prop="sex"
        label="性别"
        width="150">
      </el-table-column>
      <el-table-column
        prop="email"
        label="邮箱">
      </el-table-column>
      <el-table-column label="操作" width="180">
        <template slot-scope="scope">
          <!--编辑-->
          <el-button size="mini" @click="cheackInfo(scope.$index,scope.row);dialogFormVisible = true">查看</el-button>

          <el-dialog title="用户信息详情" :visible.sync="dialogFormVisible" style="width: 70%;margin-left: 14%">
            <el-form :model="form">
              <el-form-item label="用户名称" :label-width="formLabelWidth">
                <el-input v-model="form.username" autocomplete="off" style="width: 64%" readonly=""></el-input>
              </el-form-item>
              <el-form-item label="真实姓名" :label-width="formLabelWidth">
                <el-input v-model="form.realname" autocomplete="off" style="width: 64%" readonly=""></el-input>
              </el-form-item>
              <el-form-item label="性别" :label-width="formLabelWidth">
                <el-select v-model="form.sex" placeholder="请选择..." aria-readonly="">
                  <el-option label="男" value="男"></el-option>
                  <el-option label="女" value="女"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="邮箱" :label-width="formLabelWidth">
                <el-input v-model="form.email" autocomplete="off" style="width: 64%" readonly=""></el-input>
              </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
              <el-button @click="dialogFormVisible = false">取 消</el-button>
              <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
            </div>
          </el-dialog>
          <!--删除-->
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index,scope.row);centerDialogVisible = true">删除</el-button>
          <!--<el-dialog-->
            <!--title=""-->
            <!--:visible.sync="centerDialogVisible"-->
            <!--width="30%"-->
            <!--center>-->
            <!--<span>确定删除么？</span>-->
            <!--<span slot="footer" class="dialog-footer">-->
    <!--<el-button @click="centerDialogVisible = false">取 消</el-button>-->
    <!--<el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>-->
  <!--</span>-->
          <!--</el-dialog>-->
          <!--<el-button-->
          <!--size="mini"-->
          <!--type="danger"-->
          <!--@click="handleDelete($index, row)">删除</el-button>-->
        </template>
      </el-table-column>
    </el-table>
    <!--表格部分-->

    <!--<el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage4"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="100"
      layout="total, sizes, prev, pager, next, jumper"
      :total="400">
    </el-pagination>-->
  </div>
</template>

<script>
  export default {
    name:"listUser",
    data() {
      this.$options.methods.created(this);
      return {
        dialogFormVisible_: false,
        dialogFormVisible: false,
        form: {
          username: '',
          realname: '',
          email: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        formLabelWidth: '120px',
        centerDialogVisible: false,
        input: '',
        tableData2: []
        // {
        //   userid: '1',
        //   username: '西门吹嘘',
        //   realname:'张三',
        //   sex: '男',
        //   email:'657885688@qq.com',
        //   tel:'123456897'
        // },{
        //   userid: '1',
        //   username: '西门吹嘘',
        //   realname:'张三',
        //   sex: '男',
        //   email:'657885688@qq.com',
        //   tel:'123456897'
        // },{
        //   userid: '1',
        //   username: '西门吹嘘',
        //   realname:'张三',
        //   sex: '男',
        //   email:'657885688@qq.com',
        //   tel:'123456897'
        // },{
        //   userid: '1',
        //   username: '西门吹嘘',
        //   realname:'张三',
        //   sex: '男',
        //   email:'657885688@qq.com',
        //   tel:'123456897'
        // }

      }
    },
    methods: {
      created(id){
        // console.log("哈哈哈哈哈哈哈哈哈哈哈哈哈");
        var that=id;
        that.$axios.get('/api/manage/userController/queryUser.do')
          .then(function (resp) {
            // console.log(resp.data);
            that.tableData2=resp.data;
          })
      },
      tableRowClassName(){

      },
    //==============查看=============
      cheackInfo(index,row){
        this.form.username=row.username;
        this.form.realname=row.realname;
        this.form.sex=row.sex;
        this.form.email=row.email;
      },
      //===================删除================
      handleDelete(index, row) {
        // console.log(row.user_id);
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          let myId=row.user_id;
          this.$axios.get('/api/manage/userController/deleteUser.do', {
            params:{
              myId
            }
          })
            .then(function (resp) {
              console.log("成功");
            })
            .catch(function (err) {
              console.log(err)
            });
          this.$options.methods.created(this);
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });

      },
      //===================查询================
      searchUser(){
        let searchName=this.input;
        console.log(searchName);
        let that=this;
        that.$axios.get('/api/manage/userController/searchUser.do',{
          params:{
            searchName
          }
        })
          .then(function (resp) {
            console.log(resp.data);
            that.tableData2=resp.data;

          })
          .catch(function (err) {
          console.log(err)
        });
      }
    },

  }
</script>

<style scoped>
  .N{
    width: 260px;
    float: left;
    margin-top: 30px;
    margin-bottom: 30px;
  }
  .C{
    margin-top: 34px;
    margin-left: -1000px;
  }
</style>
