<template>
  <div class="list_wrapper" ref="list_ref" @scroll="scroll_handler">
    <div class="item" v-for="(item,index) in list" :key="index" :style="item">{{ index }}</div>
    <div class="loading" v-if="isLoading">加载中...</div>
  </div>
</template>

<script setup lang="ts">
import { ref,onMounted } from 'vue'
const list = ref([{
  background: 'rgb(233,32,38)'
}])
const list_ref = ref()
const timer = ref()
const isLoading = ref(false)
// 随机颜色
const rgb_ = () => {
  const r = Math.floor(Math.random()*256);
  const g = Math.floor(Math.random()*256);
  const b = Math.floor(Math.random()*256);
  return `rgb(${r},${g},${b})`;
}
// 获取
const getList = (num:Number) => {
  // 正在请求
  isLoading.value = true
  timer.value = setTimeout(() => {
    // 请求完毕
    isLoading.value = false
    for(let i=0;i<num;i++){
      if(list.value.length>=50){
        clearTimeout(timer.value)
        timer.value = null
        break
      }else{
        list.value.push({ background: rgb_() })
      }
    }
  },Math.ceil(Math.random()*5)*1000)
}
// 滚动
const scroll_handler = () => {
  let scroll_top = list_ref.value.scrollTop
  top.value = scroll_top
  if(scroll_top > (list.value.length*500 / 2) && list.value.length < 50){
    !isLoading.value && getList(10)
  }
}
onMounted(() => {
  getList(10)
})
</script>

<style scoped>
.list_wrapper{
  width: 500px;
  height: 100%;
  overflow-y: auto;
}
.list_wrapper .item{
  height: 500px;
  line-height: 500px;
  font-size: 30px;
  text-align: center;
}
.loading{
  font-weight: bold;
  text-align: center;
}
</style>
