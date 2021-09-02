<template>
  <div>
    <el-card>
    <Buttons :btns="cateBtn" @outclick="cateClick">
      <template v-slot:cateadd>
        <el-dialog title="添加商品" :visible.sync="visibleOne" width="400px">
          <el-form :model="dataThree" label-position="right">
            <el-form-item v-for="(x,index) in dataThree" :key="index" :label="x.name" :required="x.show" label-width="80px">
              <el-input :type="x.type"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer">
            <el-button @click="visibleOne = false">取 消</el-button>
            <el-button type="primary" @click="visibleOne = false">确 定</el-button>
          </div>
        </el-dialog>
      </template>
      <template v-slot:catedelete>
        <el-dialog title="分类商品ID" :visible.sync="visibleTwo" width="400px">
          <div v-for="(m,i) in multipleSelection" :key="i" style="text-align:center">
            选择的第{{i+1}}个商品Id：{{m.categoryId}}
          </div>
        </el-dialog>
      </template>
    </Buttons>
      <Tables :chartData="cateCharts" :address="'/api/categories'" :paramsData="cateParam" @outselection="cateSelection">
        <el-table-column label="操作" >
          <el-popconfirm title="确定修改吗？">
            <el-button slot="reference" type="text" style="margin-right:10px">修改</el-button>
          </el-popconfirm>
          <el-popconfirm title="确定下级分类吗？">
            <el-button slot="reference" type="text" style="margin-right:10px">下级分类</el-button>
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
  name: 'Category',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      multipleSelection:[], 
      visibleOne:false,
      visibleTwo:false,
      dataThree:[{name:'商品名称',show:true,type:'text'},
                 {name:'排序值',show:true,type:'number'}],
      cateBtn:[{
        name:"增加",
        icon:"el-icon-plus",
        bgColor:"#1baeae",
        color:"#fff",
        label:"cateadd"
      },{
        name:"批量删除",
        icon:"el-icon-delete",
        bgColor:"#f56c6c",
        color:"#fff",
        label:"catedelete"
      }],
      cateParam:{
          configType: '',
          orderNo: '',
          orderStatus: '',
          level:1,
          parent_id:0
      },
      cateCharts:[
        {type:"selection", width:"55"},
        {name:"categoryName", label:"分类名称"},
        {name:"categoryRank", label:"排序值"},
        {name:"createTime", label:"添加时间"},
      ]
    }
  },
  methods: {
    cateClick(index){
      if(index==0){
        this.visibleOne=true
      }
      else{
       this.visibleTwo=true
      }
    },
    cateSelection(val){
      this.multipleSelection=val
    }
  }
}
</script>

