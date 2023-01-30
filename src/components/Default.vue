<template>
  <div>
    哈哈哈
    <button @click="change">change</button>
    <h2>计算属性 computed</h2>
    姓：<input type="text" v-model="person.fName" />
    <br />
    名：<input type="text" v-model="person.lName" />
    {{ person.fullName }}

    <h2>watch</h2>
    <div>sum: {{ sum }}</div>
    <div>newMsg: {{ newMsg }}</div>
    <div>sObj: {{ sObj.sex }}</div>
    <button @click="sum++">+1</button>
    <button @click="newMsg += '-'">newMsg</button>
    <button @click="sObj.sex += '-'">change sObj.sex</button>
    <hr />
    <div>obj.info: {{ obj.info }}</div>
    <div>obj.index: {{ obj.index }}</div>
    <div>obj.childObj.j1.job:{{ obj.childObj.j1.job }}</div>
    <button @click="obj.index++">obj.index+1</button>
    <button @click="obj.info += '-'">change obj.info</button>
    <button @click="obj.childObj.j1.job += '-'">
      change obj.childObj.j1.job
    </button>
    <hr />
    <div>aa: {{ aa }}</div>
    <div>b: {{ b }}</div>
    <button @click="changeA">change a</button>
  </div>
</template>

<script>
import { computed, reactive, ref, toRef, toRefs } from "@vue/reactivity";
import { watch, watchEffect } from "@vue/runtime-core";
export default {
  name: "Default",
  props: ["msg"],
  emits: ["change"],
  setup(props, context) {
    let person = reactive({
      fName: "er",
      lName: "xun",
    });

    person.fullName = computed({
      get() {
        return person.fName + person.lName;
      },
    });
    function change() {
      context.emit("change", "吼吼吼吼 ");
    }
    /**
     * watch 监听使用
     * vue bug: 1.监听 reactive 定义的响应式数据时，oldValue 无法获取，强制开启了深度监听，即 deep 无效；
     *          2.监听 reactive 定义响应式数据内的某个属性时(非基本类型数据)，deep 有效；
     */
    let sum = ref(0);
    let newMsg = ref("缓慢");
    let sObj = ref({
      sex: "male",
    });
    // 1. watch,监听ref所定义的单个数据
    watch(
      sum,
      (newValue, oldValue) => {
        console.log("监听ref所定义的单个数据", newValue, oldValue);
      },
      { immediate: false }
    );
    // 2. watch,监听ref所定义的多个数据
    watch([sum, newMsg], (newValue, oldValue) => {
      console.log("监听ref所定义的多个数据", newValue, oldValue);
    });
    // 注意： 2.1 watch,监听ref所定义的对象
    watch(sObj.value, (newValue, oldValue) => {
      console.log("注意：监听ref所定义的对象", newValue, oldValue);
    });
    // or
    watch(
      sObj,
      (newValue, oldValue) => {
        console.log("注意：监听ref所定义的对象", newValue, oldValue);
      },
      { deep: true }
    );

    // 3. watch,监听 reactive 所定义的对象
    let obj = reactive({
      info: "嗯哼",
      index: 1,
      childObj: {
        j1: {
          job: "工程师",
        },
      },
    });
    watch(
      obj,
      (newValue, oldValue) => {
        /**
         * 注意(vue bug): 监听对象时，此时 oldValue  与 newValue 值相同，即取不到 vue2 中的旧值
         */
        console.log(
          "监听 reactive 所定义的对象",
          newValue,
          oldValue,
          newValue === oldValue
        );
      },
      {
        deep: false, // 此时 deep 无效，强制开启深度监听
      }
    );

    // 4. watch,监听 reactive 所定义对象内的单个属性(非对象)
    watch(
      () => obj.info,
      (newValue, oldValue) => {
        console.log(
          "监听 reactive 所定义对象内的单个属性(非对象)",
          newValue,
          oldValue
        );
      }
    );
    // 5. watch,监听 reactive 所定义对象内的多个属性
    watch(
      [() => obj.info, () => obj.childObj],
      (newValue, oldValue) => {
        console.log("监听 reactive 所定义对象内的多个属性", newValue, oldValue);
      },
      { deep: true } // 此时 deep 用于 childObj 进行深度监听
    );
    // 6. 注意：watch,监听 reactive 所定义对象内的单个属性(对象)
    watch(
      () => obj.childObj,
      (newValue, oldValue) => {
        console.log(
          "监听 reactive 所定义对象内的单个属性(对象)",
          newValue,
          oldValue
        );
      },
      { deep: true } // 此时 deep 用于 childObj 进行深度监听
    );

    /**
     * watchEffect,所指定回调中用到的数据只要发生变化，则直接重新执行回调
     */
    watchEffect(() => {
      let one = obj.info;
      console.log("one", one);
    });

    /**
     * toRef 和 toRefs
     * 应用场景： 要将响应式对象中某个属性单独提供给外部对象使用且保持响应式属性
     */
    let refObj = reactive({
      a: 1,
      b: 2,
    });
    const aa = toRef(refObj, "a");
    const changeA = () => {
      refObj.a = 12;
      refObj.b = 99;
    };

    return {
      person,
      change,
      sum,
      newMsg,
      obj,
      sObj,
      refObj,
      aa,
      changeA,
      ...toRefs(refObj),
    };
  },
};
</script>

<style scoped></style>
