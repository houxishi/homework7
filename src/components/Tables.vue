<template>
  <div class="tables">
    <el-table 
    :data="tableData" 
    v-loading="loading"
    tooltip-effect="dark"
    class="t-table"
    @selection-change="handleSelectionChange">
      <el-table-column 
        v-for="(item,index) in chartData"
        :key="index"
        :label="item.label" 
        :prop="item.name"
        :type="item.type"
        :width="item.width">
	</el-table-column>
     <slot></slot>	
    </el-table>
    <!--总数超过一页，再展示分页器-->
    <el-pagination
      background
      layout="prev, pager, next"
      :total="total"
      :page-size="pageSize"
      :current-page="currentPage"
      @current-change="changePage"
    />
  </div>
</template>


<script>

import axios from '@/api/index.js'

export default {
  name:'Tables',
  props:{
    chartData:Array,
    address:String,
    paramsData:Object,
    },
  data(){
    return{
      tableData:[],
      loading:false,
      total: 0,
      currentPage: 1, 
      pageSize: 10, 
    }
  },
  created(){
    this.getData()
  },
  methods: {
     getData(){
      this.loading=true
      axios.get(this.address,{
      params: {
          pageNumber: this.currentPage,
          pageSize: this.pageSize,
          configType: this.paramsData.configType,
          orderNo: this.paramsData.orderNo,
          orderStatus: this.paramsData.orderStatus,
          categoryLevel:this.paramsData.level,
          parentId:this.paramsData.parent_id
        }
      }).then(response=>{
        this.tableData=response.data.data.list;
        this.total = response.data.data.totalCount;
        this.currentPage = response.data.data.currPage;
        this.loading=false
      })
     },
    handleSelectionChange (val) {
      this.$emit('outselection',val);
    },
    changePage(val) {
      this.currentPage = val;
      this.getData()
    }
  }  
}
</script>

<style scoped>
.tables{
    text-align: center;
}
.t-table{
    margin-top: 20px;
    margin-bottom: 20px;
    width: 100%;
}
</style>