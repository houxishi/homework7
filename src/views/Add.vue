<template>
  <div class="add">
    <el-card class="add-conent">
      <el-form :model="goodForm" :rules="rules" ref="goodForm" label-width="100px">
        <el-form-item label="商品分类" style="text-align:left">
          <el-cascader style="width:300px" :props="category" @change="handleChangeCate"></el-cascader>
        </el-form-item>
        <el-form-item label="商品名称" style="text-align:left" prop="goodsName">
          <el-input style="width: 300px" v-model="goodForm.goodsName" placeholder="请输入商品名称"></el-input>
        </el-form-item>
        <el-form-item label="商品简介" style="text-align: left">
          <el-input style="width: 300px" type="textarea" v-model="goodForm.goodsIntro" placeholder="请输入商品简介(100字)"></el-input>
        </el-form-item>
        <el-form-item  label="商品价格" style="text-align:left" prop="originalPrice">
          <el-input type="number" min="0" style="width: 300px" v-model="goodForm.originalPrice" placeholder="请输入商品价格"></el-input>
        </el-form-item>
        <el-form-item  label="商品售卖价" style="text-align: left"  prop="sellingPrice">
          <el-input type="number" min="0" style="width: 300px" v-model="goodForm.sellingPrice" placeholder="请输入商品售价"></el-input>
        </el-form-item>
        <el-form-item  label="商品库存" style="text-align: left" prop="stockNum">
          <el-input type="number" min="0" style="width: 300px" v-model="goodForm.stockNum" placeholder="请输入商品库存"></el-input>
        </el-form-item>
        <el-form-item label="商品标签" style="text-align: left" >
          <el-input style="width: 300px" v-model="goodForm.tag" placeholder="请输入商品小标签"></el-input>
        </el-form-item>
        <el-form-item label="上架状态" style="text-align: left">
          <el-radio-group v-model="goodForm.goodsSellStatus">
            <el-radio label="0">上架</el-radio>
            <el-radio label="1">下架</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="商品主图" style="text-align: left">
          <el-upload
            class="avatar-uploader"
            :action="uploadImgServer"
            accept="jpg,jpeg,png"
            :show-file-list="false"
            :on-success="handleUrlSuccess"
          >
            <img style="width: 100px; height: 100px; border: 1px solid #e9e9e9;" v-if="goodForm.goodsCoverImg"  :src="goodForm.goodsCoverImg" class="avatar">
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>
        <el-form-item label="详情内容" style="text-align: left" >
          <div ref='editor'></div>
        </el-form-item>
        <el-form-item style="text-align: left">
          <el-button type="primary" @click="submitAdd('goodForm')">立即创建</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>

import axios from '@/api/index.js'
import WangEditor from 'wangeditor'

export default {
  name: 'Add',
  components: {
   },
  data() {
  return {
      editor:null,
      defaultCate: '',      
      categoryId: '',
      goodForm: {
        goodsName: '',
        goodsIntro: '',
        originalPrice: '',
        sellingPrice: '',
        stockNum: '',
        goodsSellStatus: '0',
        goodsCoverImg:'',
        tag: ''
      },
      rules: {
        goodsName: [
          { required: true, message: '请填写商品名称', trigger: 'blur'  }
        ],
        originalPrice: [
          { required: true, message: '请填写商品价格', trigger: 'blur'  }
        ],
        sellingPrice: [
          { required: true, message: '请填写商品售价', trigger: 'blur'  }
        ],
        stockNum: [
          { required: true, message: '请填写商品库存', trigger: 'blur'  }
        ],
      },
      category: {
        lazy: true,
        lazyLoad(node, resolve) {
          const { level=0,value } = node
          axios.get('/api/categories', {
            params: {
              pageNumber: 1,
              pageSize: 1000,
              categoryLevel: level + 1,
              parentId: value || 0
            }
          }).then(response => {
            const list = response.data.data.list
            const nodes = list.map(item => ({
              value: item.categoryId,
              label: item.categoryName,
              leaf: level>1
            }))
            resolve(nodes)
          })
        }
      }}
  },  
  methods: {
    handleUrlSuccess (val){
      this.goodForm.goodsCoverImg = val.data
    },
    submitAdd(goodForm){
      this.$refs[goodForm].validate((valid) => {
        if (valid) {
          let params = {
            goodsCategoryId: this.categoryId,
            goodsCoverImg: goodForm.goodsCoverImg,
            goodsDetailContent: '',
            goodsIntro: goodForm.goodsIntro,
            goodsName: goodForm.goodsName,
            goodsSellStatus: goodForm.goodsSellStatus,
            originalPrice: goodForm.originalPrice,
            sellingPrice: goodForm.sellingPrice,
            stockNum: goodForm.stockNum,
            tag: goodForm.tag
          }
          axios.post('/api/goods', params).then(() => {
            this.$router.push({ path:'/homemana/good' });
          })
        } 
        })
     }
  },
  mounted(){
    this.editor=new WangEditor(this.$refs.editor)
    this.editor.create()
  }
}
</script>

<style scoped>
  .avatar-uploader {
    width: 100px;
    height: 100px;
    color: #ddd;
    font-size: 30px;
  }
  .avatar-uploader-icon {
    display: block;
    width: 100%;
    height: 100%;
    border: 1px solid #e9e9e9;
    padding: 32px 17px;
  }
</style>
