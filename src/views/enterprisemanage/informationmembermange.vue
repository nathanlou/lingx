<template>
  <div>
 <div class="searchD"  >
   <div >
    <span style="width:6rem;" >登录账号：</span>
    <el-input v-model="input" placeholder="请输入登录账号"  style="width: 10rem;"size="small" ></el-input>
   </div>

       <div style="display: flex;margin-left: 2rem;">
           <el-button type="primary" icon="el-icon-search" round>搜索</el-button>
       </div>

 </div>
 <div class="tableD">
	 <div class="table_headr">列表</div>
       <el-table
         ref="multipleTable" :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)"
          style="width: 100%;text-align: center;">
          <el-table-column type="expand">
            <template slot-scope="props">
              <el-form label-position="left" inline class="demo-table-expand">
                <el-form-item label="登录账号">
                  <span>{{ props.row.account }}</span>
                </el-form-item>
                <el-form-item label="企业名称">
                  <span>{{ props.row.enterPriseName }}</span>
                </el-form-item>
                <el-form-item label="权限管理">
                  <span>{{ props.row.power }}</span>
                </el-form-item>
                <el-form-item label="是否启用" >
                  <span>{{ props.row.isUser }}</span>
                </el-form-item>
                <el-form-item label="联 系 人">
                  <span>{{ props.row.contactPersion }}</span>
                </el-form-item>
                <el-form-item label="联系电话">
                  <span>{{ props.row.phone }}</span>
                </el-form-item>
                <el-form-item label="登录时间">
                  <span>{{ props.row.lastLognTime }}</span>
                </el-form-item>
              </el-form>
            </template>
          </el-table-column>
          <el-table-column
            label="登录账号"
            prop="account">
          </el-table-column>
          <el-table-column
            align="center"
            label="企业名称"
            prop="enterPriseName">
          </el-table-column>


          <el-table-column
            align="center"
            label="是否启用"
            prop="isUser">
          </el-table-column>
        <el-table-column
          width="300"
          fixed="right"
          label="操作"
          align="center"
          >
          <template slot-scope="scope">
              <el-button type="primary" plain round size="mini" @click="UpdatePwddialogVisible=true">重置密码</el-button>
              <el-button type="primary" plain round size="mini" @click="editInfodialogVisible=true">编辑</el-button>
              <el-button type="primary" plain round size="mini" @click="delDialogVisible=true">删除</el-button>
          </template>
        </el-table-column>
        </el-table>
		<el-pagination class="fy" layout="total,prev, pager, next" :total="total" background @current-change="current_change" />
    </div>


 <!--重置密码dialog -->
    <el-dialog
       title="重置用户密码"
       :visible.sync="UpdatePwddialogVisible"
       width="35%"
       >
          <p style="font-size: 16px;padding: 0 0 5px 0px; "> 账户名称 :  &nbsp;&nbsp;admin123456</p>
          <div style="display: flex;align-items: center;margin-top: 8px;">
             <span style="width: 70px;">输入密码：</span>
             <el-input placeholder="请输入密码" style="width: 200px;"></el-input>
           </div>
           <div style="display: flex;align-items: center;margin-top: 8px;">
              <span style="width: 70px;">确认密码：</span>
              <el-input placeholder="请输入确认密码" style="width: 200px;"></el-input>
            </div>

           <span slot="footer" class="dialog-footer">
             <el-button @click="UpdatePwddialogVisible = false">取 消</el-button>
             <el-button type="primary" @click="UpdatePwddialogVisible = false">重置密码</el-button>
           </span>
     </el-dialog>


  <!-- 删除对话框 -->
    <el-dialog
      title="删除提示"
      :visible.sync="delDialogVisible"
      width="25%"
      center
      >
      <span >确定qianhe账户信息么？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="delDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="delDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>


 <!-- 编辑对话框-->
     <el-dialog
        title="修改账户信息"
        :visible.sync="editInfodialogVisible"
        width="23rem"
        align="center"
        >
        <div>
          <div class="accountinfoD">
            <span class="accountinfoSpan">登录账号：</span>
            <el-input class="accountinfoinput" v-model="userInfo.account" placeholder="请输入用户账号"  ></el-input>
          </div>

          <div class="accountinfoD" style="margin-top: 0.625rem;">
            <span class="accountinfoSpan">企业名称：</span>
             <el-select v-model="userInfo.enterpriseName" placeholder="请选择" class="accountinfoinput">
                <el-option
                  v-for="item in optionsEnterPN"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
          </div>

          <div class="accountinfoD" style="margin-top: 0.625rem;">
            <span class="accountinfoSpan">权限管理：</span>
             <el-select v-model="userInfo.role" placeholder="请选择" class="accountinfoinput">
                <el-option
                  v-for="item in optionsrole"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
          </div>

          <div class="accountinfoD" style="margin-top: 0.625rem;">
            <span class="accountinfoSpan">启用状态：</span>
             <el-select v-model="userInfo.isUser" placeholder="请选择" class="accountinfoinput">
                <el-option
                  v-for="item in optionsisUser"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
          </div>

          <div class="accountinfoD"  style="margin-top: 0.625rem;">
            <span class="accountinfoSpan">联 系 人：</span>
            <el-input class="accountinfoinput" v-model="userInfo.contactPersion" placeholder="请输入联系人"  ></el-input>
          </div>

          <div class="accountinfoD"  style="margin-top: 0.625rem;">
            <span class="accountinfoSpan">联系电话：</span>
            <el-input class="accountinfoinput" v-model="userInfo.accountphone" placeholder="请输入联系电话"  ></el-input>
          </div>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="editInfodialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="editInfodialogVisible = false">确 定</el-button>
        </span>
      </el-dialog>
</div>



</template>

<script>
 export default {
    data() {
      return {
				total: 1000, // 默认数据总数
				pagesize: 10, // 每页的数据条数
				currentPage: 1, // 默认开始页面
				istag: true,
        UpdatePwddialogVisible:false,
        delDialogVisible:false,
        editInfodialogVisible:false,
         userInfo: {
                  account:'',
                  password:'',
                  enterpriseName:'',
                  contactPersion:'',
                  accountphone:"",
                  isUser:'',
                  role:'',
                },

        /* 登录账号，企业名称，权限管理，联系电话，联系人，是否启用，最后登录时间*/
         tableData: [{
                   account: 'Top001',
                   enterPriseName: '山东领先',
                   power: '信息员',
                   phone: '13462268043',
                   isUser:"是",
                   "lastLognTime":'2020-11-06',
                   contactPersion: '王一',

                 },{
                   account: 'Top002',
                   enterPriseName: '山东领先',
                   power: '信息员',
                   phone: '13462268043',
                   isUser:"是",
                   "lastLognTime":'2020-11-06',
                   contactPersion: '王一',

                 },{
                   account: 'Top003',
                   enterPriseName: '山东领先',
                   power: '信息员',
                   phone: '13462268043',
                   isUser:"是",
                   "lastLognTime":'2020-11-06',
                   contactPersion: '王一',

                 }]
      }
    },
		created() {
			this.total = this.tableData.length
		},
    methods:{
			current_change: function(currentPage) {
				this.currentPage = currentPage
			},
    }
  }
</script>

<style>
  .searchD{
     margin-left: 1.5rem;
     margin-top: -1.2rem;
     color: gray;
     display: flex;
     align-items: center;

   }
   .demo-table-expand {
      font-size: 0;
    }
    .demo-table-expand label {
      width: 90px;
      color: #99a9bf;
    }
    .demo-table-expand .el-form-item {
      margin-right: 0;
      margin-bottom: 0;
      width: 50%;
    }
    .tableD{
      margin: 1.5rem;
    }
    .accountinfoD{

      display: flex;
      align-items: center;
    }
    .accountinfoSpan{
      width: 5rem;
    }
    .accountinfoinput{
      width: 12rem;
    }

</style>
