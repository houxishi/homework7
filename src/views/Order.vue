<template>
  <div class="order">
    <el-card>
      <Buttons :btns="orderBtn" @outclick="orderClick">
        <template v-slot:binput>
          <el-input placeholder="请输入订单号"  size="small" style="margin-right: 10px"></el-input>
          <el-input placeholder="全部"  size="small" style="margin-right: 10px"></el-input>
        </template>
      <template v-slot:order>
        <el-dialog :visible.sync="visibleOne" width="400px">
          请选择项
        </el-dialog>
      </template>
      <template v-slot:orderstock>
        <el-dialog :visible.sync="visibleTwo" width="400px">
          请选择项
        </el-dialog>
      </template>
      <template v-slot:orderclose>
        <el-dialog :visible.sync="visibleThree" width="400px">
          请选择项
        </el-dialog>
      </template>
      </Buttons>
      <Tables :chartData="orderCharts" :address="'/api/orders'" :paramsData="orderParam">
        <el-table-column label="支付方式" >
          <template slot-scope="scope">
            <span v-if="scope.row.payType == 1">微信支付</span>
            <span v-else-if="scope.row.payType == 2">支付宝支付</span>
            <span v-else>未支付</span>
          </template>
        </el-table-column>
        <el-table-column label="操作" >
          <template slot-scope="scope">
          <el-popconfirm
            v-if="scope.row.orderStatus == 1"
            title="确定配货完成吗？"
          >
          <el-button slot="reference" type="text" style="margin-right: 10px">配货完成</el-button>
          </el-popconfirm>
          <el-popconfirm
            v-else-if="scope.row.orderStatus == 2"
            title="确定出库吗？"
          >
          <el-button slot="reference" type="text" style="margin-right: 10px">出库</el-button>
          </el-popconfirm>
          <el-popconfirm
            v-else
            title="确定关闭订单吗？"
          >
          <el-button slot="reference" type="text" style="margin-right: 10px">关闭订单</el-button>
          </el-popconfirm>
          <!-- <router-link :to="{ path: '/order_detail', query: { id: scope.row.orderId }}">订单详情</router-link> -->
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
  name: 'Order',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      visibleOne:false,
      visibleTwo:false,
      visibleThree:false,
      orderBtn:[{
        name:"配货完成",
        icon:"el-icon-sell",
        bgColor:"#1baeae",
        color:"#fff",
        label:"order"
      },{
        name:"出库",
        icon:"el-icon-sell",
        bgColor:"#1baeae",
        color:"#fff",
        label:"orderstock"
      },{
        name:"关闭订单",
        icon:"el-icon-delete",
        bgColor:"#f56c6c",
        color:"#fff", 
        label:"orderclose" 
      }],
      orderParam:{
          configType: '',
          orderNo: '',
          orderStatus: '',
          level:'',
          parent_id:''
      },
      orderCharts:[
        {type:"selection", width:"55"},
        {name:"orderNo", label:"订单号"},
        {name:"totalPrice", label:"订单总价"},
        {name:"createTime", label:"添加时间"},
      ],
    }
  },
  methods: {
    orderClick(index){
      if(index==0){
        this.visibleOne=true
      }
      else if(index==1){
       this.visibleTwo=true
      }
      else{
       this.visibleThree=true 
      }
    }
  }
}
</script>
