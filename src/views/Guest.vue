<template>
  <div class="guest">
    <el-card>
    <Buttons :btns="btnData" @outclick="guestClick">
      <template v-slot:guestSolve>
        <el-dialog :visible.sync="visibleOne" width="400px">
          是否要禁用？
        </el-dialog>
      </template>
      <template v-slot:guestForbid>
        <el-dialog :visible.sync="visibleTwo" width="400px">
          是否要解除禁用？
        </el-dialog>
      </template>
    </Buttons>
      <Tables :chartData="guestCharts" :address="'/api/users'" :paramsData="guestParam">
        <el-table-column label="身份状态" >
          <template slot-scope="scope">
            <span :style="scope.row.lockedFlag == 0 ? 'color: green;' : 'color: red;'">
            {{ scope.row.lockedFlag == 0 ? '正常' : '禁用' }}
            </span>
          </template>
        </el-table-column>
        <el-table-column label="是否注销">
          <template slot-scope="scope">
            <span :style="scope.row.lockedFlag == 0 ? 'color: green;' : 'color: red;'">
            {{ scope.row.isDeleted == 0 ? '正常' : '注销' }}
            </span>
          </template>
        </el-table-column>
      </Tables>
    </el-card>
  </div>
</template>

<script>

import Buttons from '../components/Buttons.vue'
import Tables from '../components/Tables.vue'

export default {
  name: 'Guest',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      visibleOne:false,
      visibleTwo:false,
      btnData:[{
        name:"解除禁用",
        icon:"el-icon-plus",
        bgColor:"#1baeae",
        color:"#fff",
        label:"guestSolve"
      },{
        name:"禁用账户",
        icon:"el-icon-delete",
        bgColor:"#f56c6c",
        color:"#fff", 
        label:"guestForbid"
      }],
      guestParam:{
          configType: '',
          orderNo: '',
          orderStatus: '',
          level:'',
          parent_id:''
      },
      guestCharts:[
        {type:"selection", width:"55"},
        {name:"nickName", label:"昵称"},
        {name:"loginName", label:"登录名"},
        {name:"createTime", label:"注册时间"},
      ]
    }
  },
  methods: {
    guestClick(index){
      if(index==0){
        this.visibleOne=true
      }
      else{
       this.visibleTwo=true
      }
    }
  }
}
</script>

