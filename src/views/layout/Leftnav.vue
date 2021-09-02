<template>
  <div class="l-menu">
    <div style="height:50px;margin-left:10px">
      <img src="https://s.weituibao.com/1582958061265/mlogo.png" alt="logo">
      <span style="font-size:20px;line-height:50px;color:#fff">vue3 admin</span>
    </div>
    <el-menu 
    v-for="v in menus" 
    :key="v.name" 
    :router="true" 
    :default-active="v.path"
    text-color="#ffffff"
    background-color="#222832"
    style="width:100%">
      <el-menu-item :index="v.path" v-if="!v.children || v.children.length === 0">
        {{v.meta.title}}
      </el-menu-item>
      <el-submenu :index="v.path" v-else>
        <span slot="title">
          <span>{{v.meta.title}}</span>
        </span>
      <el-menu-item :index="v.path+'/'+ w.path" :key="w.name" v-for="w in v.children">
          <i :class="w.meta.icon"></i>
            {{w.meta.title}}
        </el-menu-item>
      </el-submenu>
      </el-menu>
  </div>
</template>

<script>

// import menus from '../../../../config/menus'
// import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
import asyncRouter  from '@/router/dynamic.js'

export default {
  name: 'Leftnav',
  data () {
    return {
      menus: asyncRouter,
    }
  },
  methods: {

  }
}
</script>

<style scoped>

.l-menu {
  width: 230px;
  min-height: 100%;
  background-color:#222832;
}
img{
  width: 50px;
  height: 50px;
  margin-right: 10px;
  float: left;
}

</style>
