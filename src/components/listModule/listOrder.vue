<template>
    <div id="app">
      <el-breadcrumb separator-class="el-icon-arrow-right"><!--面包屑导航-->
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>订单列表</el-breadcrumb-item>
      </el-breadcrumb><!--面包屑导航结束-->

      <el-row><!--查询输入框开始-->
        <el-col :span="12">
          <div class="grid-content bg-purple">
            <!--添加输入查询框-->
            <div style="margin-top: 30px;">
              <el-input placeholder="请输入内容" v-model="input5" class="input-with-select">
                <el-select v-model="select" slot="prepend" placeholder="请选择">
                  <el-option label="订单号" value="1"></el-option>
                  <el-option label="入住人" value="2"></el-option>
                  <el-option label="联系方式" value="3"></el-option>
                </el-select>
                <el-button slot="append" icon="el-icon-search"></el-button>
              </el-input>
            </div>
          </div>
        </el-col>
        <!--输入查询框结束-->

        <!--弹出表单增加订单-->
        <el-col :span="12">
          <div class="grid-content bg-purple-light">

            <el-button type="small" @click="hotelFormVisible = true" style="margin:7% 0 0 50%;">增加订单</el-button>

            <!--外层酒店订单弹框开始-->
            <el-dialog title="酒店的基本信息表" :visible.sync="hotelFormVisible">

              <el-form :model="form" ref="form">
                <el-form-item label="酒店" :label-width="formLabelWidth"><!--选择酒店-->
                  <el-select v-model="form.hotel" placeholder="请选择酒店">
                    <el-option label="常青谷" value="常青谷"></el-option>
                    <el-option label="常青堡" value="常青堡"></el-option>
                  </el-select>
                </el-form-item>

                <el-form-item label="房型" :label-width="formLabelWidth"><!--选择房型-->
                  <el-select v-model="form.room_type_name" placeholder="请选择房型">
                    <el-option label="夯土小屋" value="夯土小屋"></el-option>
                    <el-option label="一方别墅" value="一方别墅"></el-option>
                  </el-select>
                </el-form-item>

                <el-form-item label="房屋数量" :label-width="formLabelWidth"><!--选择数量-->
                  <el-input-number v-model="form.room_number" controls-position="right" @change="handleChange" :min="1" :max="10"></el-input-number>
                </el-form-item>

                <el-form-item label="套餐类型" :label-width="formLabelWidth"><!--选择套餐类型-->
                  <el-select v-model="form.consume" placeholder="请选择套餐类型">
                    <el-option label="秋叶套餐" value="秋叶套餐"></el-option>
                    <el-option label="骑行套餐" value="骑行套餐"></el-option>
                  </el-select>
                </el-form-item>

                <el-form-item label="入住时期"><!--入住的时间段-->
                  <el-date-picker
                    v-model="form.in_date"
                    type="date"
                    placeholder="选择日期">
                  </el-date-picker>
                </el-form-item><!--入住的时间段结束-->

                <el-form-item label="离开时期"><!--离开的时间-->
                  <el-date-picker
                    v-model="form.out_date"
                    type="date"
                    placeholder="选择日期">
                  </el-date-picker>
                </el-form-item><!--入住的时间段结束-->

                <el-form-item label="入住房号" :label-width="formLabelWidth"><!--选择入住房号-->
                  <el-select v-model="form.roomHao" placeholder="请选择房号">
                    <el-option label="gu_1001" value="gu_1001"></el-option>
                    <el-option label="gu_1011" value="gu_1011"></el-option>
                  </el-select>
                </el-form-item>
              </el-form>
              <!--外层的内容-->

              <!-- 内层的弹框 -->
              <el-dialog
                width="50%"
                title="入住人的基本信息表"
                :visible.sync="guestInfoFormVisible"
                append-to-body>

                <el-form :model="form" ref="form">

                  <!--酒店信息-->

                    <el-form-item label="酒店名称">
                      <span>{{ form.hotel }}</span>
                    </el-form-item>
                    <el-form-item label="房屋类型">
                      <span>{{ form.room_type_name }}</span>
                    </el-form-item>
                    <el-form-item label="房屋数量">
                      <span>{{form.room_number}}</span>
                    </el-form-item>
                    <el-form-item label="套餐">
                      <span>{{form.consume}}</span>
                    </el-form-item>
                    <el-form-item label="入住日期">
                      <span>{{ form.in_date }}</span>
                    </el-form-item>
                    <el-form-item label="离开日期">
                      <span>{{form.out_date}}</span>
                    </el-form-item>
                    <el-form-item label="房间号">
                      <span>{{ form.roomHao }}</span>
                    </el-form-item>

                  <!--酒店信息-->

                  <el-form-item label="入住者名称" :label-width="formLabelWidth"><!--入住者名称-->
                    <el-input v-model="form.guestname" autocomplete="off"></el-input>
                  </el-form-item>

                  <el-form-item label="入住者性别" :label-width="formLabelWidth"><!--选择入住人性别-->
                    <el-select v-model="form.sex" placeholder="请选择性别">
                      <el-option label="男" value="man"></el-option>
                      <el-option label="女" value="woman"></el-option>
                    </el-select>
                  </el-form-item>

                  <el-form-item label="入住者邮件" :label-width="formLabelWidth"><!--入住者联系方式-->
                    <el-input v-model="form.guest_email" autocomplete="off"></el-input>
                  </el-form-item>

                  <el-form-item label="入住者联系方式" :label-width="formLabelWidth"><!--入住者联系方式-->
                    <el-input v-model="form.guest_tel" autocomplete="off"></el-input>
                  </el-form-item>

                  <el-form-item label="成人人数" :label-width="formLabelWidth"><!--成人人数-->
                    <el-input-number v-model="form.adultNum" controls-position="right" @change="handleChange" :min="0" :max="10"></el-input-number>
                  </el-form-item>

                  <el-form-item label="儿童人数" :label-width="formLabelWidth"><!--儿童人数-->
                    <el-input-number v-model="form.childNum" controls-position="right" @change="handleChange" :min="0" :max="10"></el-input-number>
                  </el-form-item>

                  <el-form-item label="联系人名称" :label-width="formLabelWidth"><!--入住者名称-->
                    <el-input v-model="form.contact_name" autocomplete="off"></el-input>
                  </el-form-item>

                  <el-form-item label="联系人电话" :label-width="formLabelWidth"><!--入住者联系方式-->
                    <el-input v-model="form.contact_phone" autocomplete="off"></el-input>
                  </el-form-item>
                  <el-form-item label="特殊需求" :label-width="formLabelWidth"><!--备注-->
                    <el-input
                      type="textarea"
                      autosize
                      placeholder="请输入内容"
                      v-model="form.specials">
                    </el-input>
                  </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                  <el-button @click="guestInfoFormVisible = false">取 消</el-button>
                  <el-button type="primary" @click="submitAddForm('form')">确 定</el-button>
                </div>
              </el-dialog>
              <!--<template slot-scope="scope">-->
                <!--<el-button size="mini" @click="submitHotelForm(scope.$index, scope.row);EditDialogFormVisible = true">编辑</el-button>-->
                <!--<el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>-->
              <!--</template>-->
              <div slot="footer" class="dialog-footer">
                <el-button @click="hotelFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="guestInfoFormVisible = true">确 定</el-button>
              </div>
            </el-dialog>
            <!--外层酒店订单表单结束-->
          </div>
        </el-col>
        <!--弹出表单增加订单按钮结束-->
      </el-row>


      <!--表格的内容部分-->
      <el-table
        :data="tableData"
        style="width: 100%;
        margin-top: 15px;">
        <el-table-column
          fixed
          prop="order_hao"
          label="订单号"
          width="180">
        </el-table-column>
        <el-table-column
          prop="room_type_name"
          label="类型名称"
          width="120">
        </el-table-column>
        <el-table-column
          prop="room_number"
          label="房屋数量"
          width="80">
        </el-table-column>
        <el-table-column
          prop="guest_tel"
          label="入住人联系方式"
          width="120">
        </el-table-column>
        <el-table-column
          prop="guestname"
          label="入住人"
          width="120">
        </el-table-column>
        <el-table-column
          prop="order_state"
          label="订单状态"
          width="120">
        </el-table-column>
        <el-table-column
          prop="order_time"
          label="下单时间"
          width="250">
        </el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="180">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!--表格的内容部分结束-->

      <!--删除的弹框-->
      <el-dialog
        title=""
        :visible.sync="centerDialogVisible"
        width="30%"
        center>
        <span>确定删除么？</span>
        <span slot="footer" class="dialog-footer">
    <el-button @click="centerDialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
  </span>
      </el-dialog>
      <!--删除的弹框结束-->

      <!--编辑订单弹框开始-->
      <el-dialog title="编辑订单" :visible.sync="dialogFormVisible1">
        <el-form :model="form" ref="form">

          <el-form-item label="类型名称" :label-width="formLabelWidth"><!--选择房型-->
            <el-select v-model="form.room_type_name" placeholder="请选择房型">
              <el-option label="夯土小屋" value="hangtu"></el-option>
              <el-option label="一方别墅" value="yifang"></el-option>
            </el-select>
          </el-form-item>

          <el-form-item label="房屋数量" :label-width="formLabelWidth"><!--选择数量-->
            <el-input-number v-model="form.room_number" controls-position="right" @change="handleChange" :min="1" :max="10"></el-input-number>
          </el-form-item>

          <el-form-item label="入住者名称" :label-width="formLabelWidth"><!--入住者名称-->
            <el-input v-model="form.guestname" autocomplete="off"></el-input>
          </el-form-item>

          <el-form-item label="入住者联系方式" :label-width="formLabelWidth"><!--入住者联系方式-->
            <el-input v-model="form.guest_tel" autocomplete="off"></el-input>
          </el-form-item>

          <el-form-item label="订单状态" :label-width="formLabelWidth"><!--选择房型-->
            <el-select v-model="form.order_state" placeholder="请选择状态">
              <el-option label="已完成" value="finlish"></el-option>
              <el-option label="已取消" value="cancel"></el-option>
            </el-select>
          </el-form-item>

        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible1 = false">取 消</el-button>
          <el-button type="primary" :plain="true" @click="submit_Form('form')">编 辑</el-button>
        </div>
      </el-dialog>
      <!--编辑弹框结束-->

      <div class="pageDiv"><!--分页-->
        <el-pagination
          background
          layout="prev, pager, next"
          :total="1000"><!--分页的功能-->
        </el-pagination>
      </div>


    </div>
</template>

<script>
    export default {
        name: "listOrder",
      methods: {
        //====编辑框显示=====//
        handleEdit(index, row) {
          console.log("来到编辑对应信息部分");
          this.dialogFormVisible1 = true;
          this.form.order_id=row.order_id;
          this.form.room_type_name=row.room_type_name;
          this.form.room_number=row.room_number;
          this.form.guestname=row.guestname;
          this.form.guest_tel=row.guest_tel;
          this.form.order_state=row.order_state;
        },
        //======酒店表单提交====//
        // submitHotelForm(form){
        //   console.log("=========================酒店提交表单==========================")
        //   console.log(this.form);
        // },
        //=====删除操作======//
        handleDelete(index, row) {
          // console.log(index, row);
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$axios.get('/api/deleteorder.do', {
              params:{
                order_id:row.order_id,
              }
            })
              .then(function (resp) {
                console.log("成功");
              })
              .catch(function (err) {
                console.log(err)
              });
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

        handleChange(value) {
          console.log(value);
        },
        //======编辑提交=======//
        submit_Form(form){
          this.$refs[form].validate((valid)=>{
            if(valid){
              this.$axios.get('/api/editorder.do',{
              params:{
                order_id:this.form.order_id,
                room_type_name:this.form.room_type_name,
                room_number:this.form.room_number,
                guestname:this.form.guestname,
                guest_tel:this.form.guest_tel,
                order_state:this.form.order_state
              }
              }).then(function (res) {
                console.log("ok")
              }).catch(function (err) {
                console.log(err)
              });

              this.dialogFormVisible1=false;

            }else {
              return false
            }
          })
        },
        submitAddForm(form){
            this.$refs[form].validate((valid)=>{
              if(valid){
                this.$axios.get('/api/addorder.do',{
                  params:{
                    hotel:this.form.hotel,
                    room_type_name:this.form.room_type_name,
                    room_number:this.form.room_number,
                    consume:this.form.consume,
                    in_date:this.form.in_date,
                    out_date:this.form.out_date,
                    roomHao:this.form.roomHao,

                    guestname:this.form.guestname,
                    guest_sex:this.form.guest_sex,
                    guest_tel:this.form.guest_tel,
                    guest_email:this.form.guest_email,
                    adultNum:this.form.adultNum,
                    childNum:this.form.childNum,
                    contact_name:this.form.contact_name,
                    contact_phone:this.form.contact_phone,
                    specials:this.form.specials,

                    order_hao:this.form.order_hao,

                    order_time:this.form.order_time,

                    room_consume_type_id:this.form.room_consume_type_id,
                    user_id:this.form.user_id,
                    order_state:this.form.order_state,
                    subscription:this.form.subscription,
                    all_money:this.form.all_money,
                    pay:this.form.pay,
                    tax:this.form.tax,
                    server_money:this.form.server_money

                  }
                }).then(function (res) {
                  console.log("ok")
                }).catch(function (err) {
                  console.log(err)
                });

                this.hotelFormVisible=false;
                this.guestInfoFormVisible=false;

              }else {
                return false
              }
            })

        },
        getlocalTime(){
          return date('YmdHis');
        }

        },
      data(){
          return{
            select:'',
            input5:'',
            tableData:[]
            ,
            form: {
              order_id:'',
              hotel:'',
              room_type_name:'',
              room_number:'1',
              consume:'',
              roomHao:'',
              in_date:'',
              out_date:'',

              guestname:'',
              guest_tel:'',
              guest_email:'',
              adultNum:'1',
              childNum:'1',
              contact_name:'',
              contact_phone:'',
              sex:'',
              specials:'',

              order_state:'已完成',//默认已完成
              order_hao:'2018120720287508',//自动生成
              order_time:'',//获取当前时间磋
              room_consume_type_id:'1',//获取房间价格类型
              user_id:'1',//获取当前用户id
              subscription:'NULL',//默认null
              all_money:'NULL',//默认生成

              pay:'NULL',//
              tax:'NULL',//默认生成
              server_money:'NULL',//默认生成

            },
            hotelFormVisible:false,
            guestInfoFormVisible:false,
            dialogFormVisible1:false,
            centerDialogVisible:false,
            formLabelWidth: '120px'
          }
      },
      created() {
        console.log("进入create");
        var that=this;
        this.$axios.get('/api/getorder.do').then(function (resp) {
          that.tableData=resp.data;
        })
      },

    }
</script>

<style scoped>
  .el-select .el-input {
    /*width: 130px;*/
  }
  .pageDiv{
    margin-top: 100px;
  }
  .input-with-select .el-input-group__prepend {
    background-color: #fff;
  }

</style>
