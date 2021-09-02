<template>
  <div class="h-header">
    <span>{{title}}</span>
    <div class="h-right">
      <!-- <el-dropdown  trigger="click">
        <span class="el-dropdown-link">
          <i class="icon el-icon-s-custom"></i>
          十三
          <i class="el-icon-caret-bottom"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>
            用户名
          </el-dropdown-item>
          <el-dropdown-item>
            昵称
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown> -->
      <el-popover
        placement="bottom"
        :width="320"
        trigger="click"
        popper-class="popper-user-box"
      >
        <template slot="reference">
          <div class="author">
            <i class="icon el-icon-s-custom" />
            {{userInfo.nickName || '' }}
            <i class="el-icon-caret-bottom" />
          </div>
        </template>
        <div class="nickname">
          <p>登录名：{{userInfo.loginUserName || '' }}</p>
          <p>昵称：{{ userInfo.nickName || '' }}</p>
          <el-tag size="small" effect="dark" class="logout" @click="logout">退出</el-tag>
        </div>
      </el-popover>
    </div>
  </div>
</template>

<script>

import axios from '@/api/index.js'

export default {
  name: 'Header',
  data () {
    return {
      title:this.$route.name,
      userInfo:{}
    }
  },
  watch: {
		$route(to) {
      this.title=to.name
			}
	},
  mounted () {
    this.getUserInfo()
  },
  methods: {
    async getUserInfo(){
      const userInfo = await axios.get('/api/adminUser/profile')
      this.userInfo=userInfo.data.data
   },
   logout(){
      this.$router.push({ path:'/' });
   }
  }
}
</script>

<style scoped>
.h-header {
    /* position:fixed;
    width: 100%;
    background-color: white; */
    display: flex;
    justify-content: space-between;
    height: 50px;
    padding: 0 20px;
    line-height: 50px;
    font-size:20px;
    border-bottom: 1px solid #e9e9e9;
}
/* .h-right{
  display: flex;
} */
</style>