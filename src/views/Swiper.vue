<template>
  <div class="swiper">
    <el-card>
    <Buttons :btns="swiBtn" @outclick="swiClick">
      <template v-slot:swiadd>
        <el-dialog title="添加商品" :visible.sync="visibleOne" width="400px">
          <el-form :model="dataTwo" label-position="right">
            <el-form-item label="图片" required="true" label-width="80px">
                <i class="el-icon-plus avatar-uploader-icon"></i>
            </el-form-item>
            <el-form-item v-for="(n,index) in dataTwo" :key="index" :label="n.name" :required="n.show" label-width="80px">
                <el-input :type="n.type"></el-input>
            </el-form-item>
          </el-form>
        </el-dialog>
      </template>
      <template v-slot:swidelete>
        <el-dialog title="所选轮播图ID" :visible.sync="visibleTwo" width="400px">
          <div v-for="(m,i) in multipleSelection" :key="i" style="text-align:center">
            选择的第{{i+1}}个商品Id：{{m.carouselId}}
          </div>
        </el-dialog>
      </template>
    </Buttons>
    <Tables :chartData="swiCharts" :address="'/api/carousels'" :paramsData="swiParam" @outselection="swiSelection">
      <el-table-column label="轮播图" width="200">
        <template slot-scope="scope">
          <img style="width: 150px;height: 150px" :src="scope.row.carouselUrl" alt="轮播图">
        </template>
      </el-table-column>
      <el-table-column label="跳转链接" width="230">
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
  name: 'Swiper',
  components: {
    Buttons,
    Tables
  },
  data(){
    return {
      multipleSelection:[], 
      visibleOne:false,
      visibleTwo:false,
      dataTwo:[
        {name:'跳转链接',show:false,type:'text'},
        {name:'排序值',show:true,type:'number'}
      ],
      swiBtn:[
        {name:"增加",icon:"el-icon-plus",bgColor:"#1baeae",color:"#fff",label:"swiadd"},
        {name:"批量删除",icon:"el-icon-delete",bgColor:"#f56c6c",color:"#fff",label:"swidelete"}
      ],
      swiParam:{
          configType: '',
          orderNo: '',
          orderStatus: '',
          level:'',
          parent_id:''
      },
      swiCharts:[
        {type:"selection", width:"55"},
        {name:"carouselRank", label:"排序值"},
        {name:"createTime", label:"添加时间", width:"220"},
      ]
    }
  },
  methods: {
    swiClick(index){
      if(index==0){
        this.visibleOne=true
      }
      else{
       this.visibleTwo=true
      }
    },
    swiSelection(val){
      this.multipleSelection=val
    }
  }
}
</script>

<style scoped>
  .avatar-uploader-icon {
    display: block;
    width: 15px;
    height: 15px;
    border: 1px solid #e9e9e9;
    padding: 32px 32px;
  }
</style>
