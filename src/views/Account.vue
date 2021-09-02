<template>
<div>
  <el-card class="account-container">
    <el-form :model="nameForm" :rules="rules" ref="nameRef" label-width="80px" label-position="right" class="demo-ruleForm">
      <el-form-item label="登录名：" prop="loginName">
        <el-input style="width: 200px" v-model="nameForm.loginName"></el-input>
      </el-form-item>
      <el-form-item label="昵称：" prop="nickName">
        <el-input style="width: 200px" v-model="nameForm.nickName"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="submitName">确认修改</el-button>
      </el-form-item>
    </el-form>
  </el-card>
  <el-card class="account-container">
    <el-form :model="passForm" :rules="rules" ref="passRef" label-width="80px" label-position="right" class="demo-ruleForm">
      <el-form-item label="原密码：" prop="oldpass">
        <el-input style="width: 200px" v-model="passForm.oldpass"></el-input>
      </el-form-item>
      <el-form-item label="新密码：" prop="newpass">
        <el-input style="width: 200px" v-model="passForm.newpass"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="danger" @click="submitPass">确认修改</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</div>
</template>

<script>

import axios from '@/api/index.js'
// import md5 from 'js-md5'

export default{
  name:'Account',
  data () {
    return {
      nameForm: {
        loginName: '',
        nickName: ''
      },
      passForm: {
        oldpass: '',
        newpass: ''
      },
      rules: {
        loginName: [
          { required: 'true', message: '登录名不能为空', trigger: ['change'] }
        ],
        nickName: [
          { required: 'true', message: '昵称不能为空', trigger: ['change'] }
        ],
        oldpass: [
          { required: 'true', message: '原密码不能为空', trigger: ['change'] }
        ],
        newpass: [
          { required: 'true', message: '新密码不能为空', trigger: ['change'] }
        ]
      }
    }
  },
  mounted(){
    this.getAccount()
  },
  methods: {
    getAccount(){
       axios.get('/api/adminUser/profile').then(response => {
        // this.user = response
        this.nameForm.loginName = response.data.data.loginUserName
        this.nameForm.nickName = response.data.data.nickName
      })
    },
    
  }
}
</script>

<style scoped>
  .account-container {
    margin-bottom: 20px;
  }
</style>
