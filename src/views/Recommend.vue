<template>
  <div class="hot">
    <el-card>
    <Buttons :btns="recBtn" @outclick="recClick">
      <template v-slot:recadd>
        <el-dialog title="添加商品" :visible.sync="visibleOne" width="400px">
          <el-form :model="dataOne" label-position="right">
            <el-form-item v-for="(m,index) in dataOne" :key="index" :label="m.name" :required="m.show" label-width="80px">
              <el-input :type="m.type"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer">
            <el-button @click="visibleOne = false">取 消</el-button>
            <el-button type="primary" @click="visibleOne = false">确 定</el-button>
          </div>
        </el-dialog>
      </template>
      <template v-slot:recdelete>
        <el-dialog title="所推荐商品ID" :visible.sync="visibleTwo" width="400px">
          <div v-for="(m,i) in multipleSelection" :key="i" style="text-align:center">
            选择的第{{i+1}}个商品Id：{{m.goodsId}}
          </div>
        </el-dialog>
      </template>
    </Buttons>
    <Tables :chartData="recCharts" :address="'/api/indexConfigs'" :paramsData="recParam" @outselection="recSelection">
      <el-table-column label="跳转链接">
        <template slot-scope="scope">
          <a target="_blank" :href="scope.row.redirectUrl">{{ scope.row.redirectUrl }}</a>
        </template>
      </el-table-column>
      <el-table-column label="操作" >
        <el-popconfirm title="确定修改吗？">
          <el-button slot="reference" type="text" style="margin-right:10px">修改</el-button>
        </el-popconfirm>
          <el-popconfirm title="确定删除吗？">
              <el-button slot="reference" type="text">删除</el-button>
          </el-popconfirm>
      </el-table-column>
    </Tables>
    </el-card>
  </div>
</template>

<script>

import Buttons from '../components/Buttons.vue'
import Tables from '../components/Tables.vue'

export default {
  name: 'Recommend',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      visibleOne:false,
      visibleTwo:false,
      dataOne:[{name:'商品名称',show:true,type:'text'},
               {name:'跳转链接',show:false,type:'text'},
               {name:'商品编号',show:true,type:'number'},
               {name:'排序值',show:true,type:'number'},],
      recBtn:[{
        name:"增加",
        icon:"el-icon-plus",
        bgColor:"#1baeae",
        color:"#fff",
        label:"recadd"
      },{
        name:"批量删除",
        icon:"el-icon-delete",
        bgColor:"#f56c6c",
        color:"#fff",
        label:"recdelete"
      }],
      recParam:{
          configType: 5,
          orderNo: '',
          orderStatus: '',
          level:'',
          parent_id:''
      },
      recCharts:[
        {type:"selection", width:"55"},
        {name:"configName", label:"商品名称"},
        {name:"configRank", label:"排序值"},
        {name:"goodsId", label:"商品编号"},
        {name:"createTime", label:"添加时间", width:"220"},
      ]
    }
  },
  methods: {
    recClick(index){
      if(index==0){
        this.visibleOne=true
      }
      else{
       this.visibleTwo=true
      }
    },
    recSelection(val){
      this.multipleSelection=val
    }
  }
}
</script>