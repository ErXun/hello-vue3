<template>
<p>hi,jack,how are you?</p>
<h1>i am fine!</h1>
<h2>thank you!!!!!</h2>
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
