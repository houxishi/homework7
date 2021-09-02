<template>
  <div class="good">
    <el-card>
      <Buttons :btns="goodBtn" @outclick="goodClick"></Buttons>
      <Tables :chartData="goodCharts" :address="'/api/goods/list'" :paramsData="goodParam">
        <el-table-column label="商品图片" width="200">
          <template slot-scope="scope">
            <img style="width: 100px; height: 100px;"  :src="scope.row.goodsCoverImg" alt="商品主图">
          </template>
        </el-table-column>
        <el-table-column label="上架状态">
          <template slot-scope="scope">
            <span style="color: green;" v-if="scope.row.goodsSellStatus == 0">销售中</span>
            <span style="color: red;" v-else>已下架</span>
          </template>
        </el-table-column>
        <el-table-column label="操作" >
          <template slot-scope="scope">
            <el-popconfirm title="确定修改吗？">
              <el-button slot="reference" type="text" style="margin-right:10px">修改</el-button>
            </el-popconfirm>
            <el-button type="text" v-if="scope.row.goodsSellStatus == 0">上架</el-button>
            <el-button slot="reference" type="text" v-else>下架</el-button>
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
  name: 'Good',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      goodBtn:[{
        name:"新增商品",
        icon:"el-icon-plus",
        bgColor:"#1baeae",
        color:"#fff",
      }],
      goodParam:{
          configType: '',
          orderNo: '',
          orderStatus: '',
          level:'',
          parent_id:''
      },
      goodCharts:[
        {type:"selection", width:"55"},
        {name:"goodsId", label:"商品编号"},
        {name:"goodsName", label:"商品名"},
        {name:"goodsIntro", label:"商品简介"},
        {name:"stockNum", label:"商品库存"},
        {name:"sellingPrice", label:"商品售价"},
      ]
    }
  },
  methods: {
    goodClick(){
      this.$router.push({ path:'/homepage/add' });
    }
  }
}
</script>
