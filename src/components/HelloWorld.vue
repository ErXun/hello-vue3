<template>
<h1>呵呵</h1>   
<h2>哈哈哈哈</h2>
<h1>test</h1>
</template>

<script>
import {h} from 'vue'
export default {
  name: 'HelloWorld',
  setup(){
    // 返回一个渲染函数
    return () => h('h1','哈哈')
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
