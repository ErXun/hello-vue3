<template>
  <div>
    <h1>shallowRef 使用</h1>
    <div>rObj.name: {{ rObj.name }}</div>
    <div>sCount: {{ sCount }}</div>
    <button @click="change">change rObj.name</button>
    <hr />
    <h1>shallowReactive 使用</h1>
    <div>state.one: {{ state.one }}</div>
    <div>state.cObj.cOne: {{ state.cObj.cOne }}</div>
    <button @click="state.one++">change state.one</button>
    <button @click="changeCObj">change state.cObj.cOne</button>
    <hr />
    <h1>shallowReadonly 使用</h1>
    <div>srState.one: {{ srState.one }}</div>
    <div>srState.cObj.cOne: {{ srState.cObj.cOne }}</div>
    <button @click="changeSrState">changeSrState</button>
    <hr />
    <h1>customRef 使用</h1>
    <input type="text" v-model="iName" />
    <div>{{ iName }}</div>
  </div>
</template>

<script>
import {
  shallowRef,
  ref,
  triggerRef,
  shallowReactive,
  shallowReadonly,
  isReadonly,
  customRef,
} from "@vue/reactivity";
import {
  onBeforeMount,
  onMounted,
  watch,
  watchEffect,
} from "@vue/runtime-core";
export default {
  name: "",
  setup() {
    let count = ref(0);
    let obj = ref({
      name: "小米",
      age: 18,
    });
    /**
     * 1. shallowRef ,只有对 .value 的访问是响应式的，内部值不会被深层递归转为响应式
     */
    let sCount = shallowRef(0);
    let rObj = shallowRef({
      name: "小米",
      age: 18,
    });
    function change() {
      sCount.value++; // 响应式
      rObj.value = { name: "大米" }; // 响应式
      // rObj.value.name = "大米";  // 数据改变，但页面不刷新
      // console.log("rObj.value", rObj, rObj.value);
    }

    /**
     * 2. triggerRef,强制触发依赖于一个浅层 ref 的副作用
     * 应用场景： 通常在对浅引用的内部值进行深度变更后使用。
     */
    const shallow = shallowRef({
      greet: "Hello, world",
    });
    // 触发该副作用第一次应该会打印 "Hello, world"
    watchEffect(() => {
      console.log("triggerRef", shallow.value.greet);
    });
    // 这次变更不应触发副作用，因为这个 ref 是浅层的
    shallow.value.greet = "Hello, universe";
    // 打印 "Hello, universe"
    triggerRef(shallow);

    /**
     * 3. shallowReactive,浅层响应式，同上
     */
    const state = shallowReactive({
      one: 1,
      two: 2,
      cObj: {
        cOne: 1,
      },
    });
    function changeCObj() {
      state.cObj.cOne++;
      console.log("state.cObj.cOne", state.cObj.cOne);
    }

    /**
     * 4.shallowReadonly,更改自身属性失败，更改下层嵌套属性会成功
     */

    const srState = shallowReadonly({
      one: 1,
      two: 2,
      cObj: {
        cOne: 1,
      },
    });
    function changeSrState() {
      isReadonly(srState.cObj, "cObj");
      // srState.one = 3;
      srState.cObj.cOne++;
      console.log("------srState", srState);
    }

    /**
     * customsRef,自定义一个 ref
     */

    function myRef(value) {
      return customRef((track, trigger) => {
        return {
          get() {
            console.log(`读取数据,${value}`);
            track(); // 通知 vue 去追踪 value 的变化
            return value;
          },
          set(newValue) {
            console.log("修改数据了。。。");
            value = newValue;
            trigger(); // 通知 vue 去重新解析模板
          },
        };
      });
    }

    let iName = myRef("erxun");
    watch(iName, (newValue) => {
      console.log("---newValue", newValue);
    });

    onBeforeMount(() => {
      console.log("onBeforeMount");
    });
    onMounted(() => {
      console.log("onMounted");
    });

    return {
      count,
      sCount,
      obj,
      rObj,
      change,
      shallow,
      state,
      changeCObj,
      srState,
      changeSrState,
      iName,
    };
  },
  beforeMount() {
    console.log("beforeMount");
  },
  mounted() {
    console.log("mounted");
  },
};
</script>
<style scoped></style>
