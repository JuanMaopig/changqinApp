<template>
  <!--房间列表-->
  <div class="roomList">
    <!--表头-->
    <el-row type="flex" :gutter="15">
      <!--房间搜索-->
      <el-col :xs="4" :sm="5" :md="6" :lg="7" :xl="8">
        <div class="grid-content bg-purple">
          <el-input v-model="input1" class="input" placeholder="房间号"></el-input>
        </div>
      </el-col>
      <!--查询按钮-->
      <el-col :xs="2" :sm="2" :md="2" :lg="3" :xl="4">
        <div class="grid-content bg-purple">
          <el-button type="primary" plain  @click="newRoom(this)">查询</el-button>
        </div>
      </el-col>
      <!--新增按钮-->
      <el-col :xs="2" :sm="2" :md="2" :lg="3" :xl="4" :offset="9">
        <div class="grid-content bg-purple">
          <el-button type="primary" @click="dialogVisible = true">新增</el-button>
        </div>
      </el-col>
    </el-row>
    <br>
    <br>
    <br>
    <!--表头-->
    <el-table :data="roomtableData" border style="width: 100%">
        <el-table-column prop="room_id" label="房间号" width="180">
        </el-table-column>
        <el-table-column prop="door_hao" label="类型名称" width="180">
        </el-table-column>
        <el-table-column prop="room_type_id" label="酒店名称">
        </el-table-column>
        <el-table-column prop="room_state" label="状态">
        </el-table-column>
        <el-table-column prop="isactive" label="是否激活">
        </el-table-column>
      <!--按钮-->
        <el-table-column label="操作" width="180">
          <template slot-scope="scope">
            <template>
              <el-button type="primary" size="mini" @click="handleEdit(scope.$index,scope.row)">编辑</el-button>
            </template>
            <template>
              <el-button type="danger" size="mini" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
          </template>
        </el-table-column>
    </el-table>
    <!--编辑弹框-->
    <el-dialog title="房间列表编辑" :visible.sync="dialogFormVisible">
      <el-form :model="Roomform" :ref="Roomform">
        <!--酒店名称、默认-->
        <el-form-item label="酒店名称" :label-width="formLabelWidth">
          <el-select v-model="Roomform.room_type_id" placeholder="常青谷">
          </el-select>
        </el-form-item>
        <!--房间类型，默认-->
        <el-form-item  label="房间类型" :label-width="formLabelWidth">
          <el-select  v-model="Roomform.door_hao" placeholder="夯土小屋">
          </el-select>
        </el-form-item>
        <!--状态-->
        <el-form-item label="状态" :label-width="formLabelWidth">
          <el-select v-model="Roomform.room_state" placeholder="请选择">
            <el-option label="租出" value="0"></el-option>
            <el-option label="空闲" value="1"></el-option>
            <el-option label="维护" value="2"></el-option>
          </el-select>
        </el-form-item>
        <!--激活-->
        <el-form-item label="是否激活" :label-width="formLabelWidth">
          <el-select v-model="Roomform.isactive" placeholder="请选择">
            <el-option label="是" value="3"></el-option>
            <el-option label="否" value="4"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <!--按钮-->
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submit_Form('Roomform')">确 定</el-button>
      </div>
    </el-dialog>
    <!--新增房间-->
    <el-dialog title="新增房间" :visible.sync="dialogVisible" width="40%" :before-close="handleClose">
      <el-form :model="Roomform"  :ref="Roomform">
      <!--固定的房间类型-->
        <el-row>
          <el-col :span="8">
            <div class="grid-content bg-purple-dark">
              <h4>房间类型--</h4>
            </div>
          </el-col>
          <el-col :span="10">
            <div class="grid-content bg-purple-dark">
              <br>
              <template>
                <!--下拉框-->
                <el-select v-model="Roomform.value1" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
              </template>
            </div>
          </el-col>
        </el-row>
        <!--房间号-->
        <el-row>
          <el-col :span="8">
              <div class="grid-content bg-purple-dark">
                <h4>房间号：</h4>
              </div>
            </el-col>
          <el-col :span="10">
              <br>
              <div class="grid-content bg-purple-dark">
               <el-input v-model="Roomform.input" placeholder="请输房间号">
               </el-input>
              </div>
            </el-col>
        </el-row>
        <!--按钮-->
        <br>
        <br>
        <el-row>
          <el-col :span="12" :offset="7">
            <div class="grid-content bg-purple-dark">
              <span  class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="submitForm('form')">确认创建</el-button>
              </span>
            </div></el-col>
        </el-row>
      </el-form>
    </el-dialog>
  </div>
</template>
<script>
  let options= [{
      value: '选项1',
      label: '夯土小屋'
    }, {
      value: '选项2',
      label: '夯土小屋双人'
    }, {
      value: '选项3',
      label: '树顶别墅双人'
    }, {
      value: '选项4',
      label: '树顶别墅三人'
    }];
  export default {

      name: `roomList`,
      data() {
        this.$options.methods.created(this);
        return {
          input:'',
          input1:'',
          dialogVisible: false,
          roomtableData:[],
          options:options,
           dialogTableVisible: false,
          dialogFormVisible: false,
          Roomform: {
            room_type_id:'',
            door_hao: '',
            room_state:'',
            isactive: '',
            input:'',
            value1:''
          },
          formLabelWidth: '120px',
        }
      },
      methods:{
        handleClose(){},
        handleclose(){},
        //获取页面的数据
        created(id){
          id.$axios.get('/api/roomInformation.do')
            .then(function (resp) {
              // console.log("2222222");
              id.roomtableData=resp.data;
            });
        },
        //编辑弹框里面的参数
        handleEdit(index, row) {
          console.log(row);
          this.dialogFormVisible=true;
          this.Roomform.room_type_id=row.room_type_id;
          this.Roomform.door_hao=row.door_hao;
          this.Roomform.room_state=row.room_state;
          this.Roomform.isactive=row.isactive;
        },

        //删除数据
        handleDelete(index, row) {
          // console.log(index, row);
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$axios.get('/api/roomDelete.do', {
              params:{
                room_id:row.room_id
              }
            }).then(function (resp) {
                console.log("成功");
              roomtableData=resp.data;
              }).catch(function (err) {
                console.log(err)
              });
            this.$message({
              type: 'success',
              message: '删除成功!'
            })
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消删除'
            });
          });
        },

      //编辑弹框的数据
      submit_Form(form){
        console.log("1111111111111111");
        this.dialogFormVisible=false;
        this.$refs[form].validate((valid) => {
          if (valid) {
            //编辑部门表单传数据

            this.$axios.get('/api/roomModify.do', {
              params:{
                room_type_id:this.Roomform.room_type_id,
                door_hao:this.Roomform.door_hao,
                room_state:this.Roomform.room_state,
                isactive:this.Roomform.isactive
              }
            }).then(function (res) {
                console.log("成功");
              }).catch(function (err) {
                console.log(err)
              });
          } else {
            return false;
          }
        })
      },
      //新增房间
      submitForm(Roomform) {
          var that=this;
        console.log("123456789");
        that.dialogFormVisible = false;
        that.$refs[Roomform].validate((valid) => {
          if (valid) {
            //添加部门表单传数据

            // let formData=[that.form.input,that.form.value1];
            that.$axios.get('/api/roomNew.do', {
              params:{
                room_id:row.room_id,
                door_hao:that.Roomform.value1,
                room_type_id:that.Roomform.room_type_id,
                room_state:that.Roomform.room_state,
                isactivet:that.Roomform.isactive
              }
            })
              .then(function (resp) {
                console.log("======+"+resp.data);
                if(resp.data.state=="err"){
                  console.log(resp.err);
                }
                if(resp.data.state=="ok"){
                  console.log("添加成功");
                }
                this.ruleForm.name=undefined;
                this.ruleForm.person=undefined;
              })
              .catch(function (err) {
                console.log(err);
              });


            this.EditDialogFormVisible = false;
            this.$message({
              type: 'success',
              message: '操作成功!',
            });
            this.$options.methods.created(this);
          } else {
            return false;
          }
        });
        this.$refs[Roomform].resetFields();
      },
    //房间号搜索
    newRoom(id){
      id.$axios.get('/api/selectRoom.do',{
        params:{room_id:input1}
      }).then(function (resp) {
          id.roomtableData=resp.data;
        }).catch(function (err) {
        console.log(err);
      });
    }
  }
  };
</script>

<style scoped>


</style>
