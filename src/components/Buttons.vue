<template>
  <div  style="display:flex">
    <slot name='binput'>
    </slot>
    <div  v-for="(item,index) in btns" :key="index">
      <el-button plain 
      size="small"
      :icon="item.icon" 
      :style="{backgroundColor:item.bgColor, color:item.color}"
      @click="btnClick(index)"
      style="margin-right: 10px">
      {{item.name}}
      </el-button>
      <slot :name="item.label">
      </slot>
    </div>
  </div>
</template>


<script>

export default {
  name:'Buttons',
  props:{
    btns:Array
    },
  data(){
    return{
    }
  },
  methods: {
    btnClick(i) {
    this.$emit('outclick',i);
    }
   }
}
</script>