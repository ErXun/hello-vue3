<template>
  <p>count:{{ state.count }}</p>
  <button @click="increase">++</button>
  <hr />
  <div>{{ state.info }}</div>
  <div>{{ state.hobbies }}</div>

  <div v-for="(value, key, index) in state.info" :key="index">
    {{ value }} --- {{ key }} --- {{ index }}
  </div>

  <button @click="changeInfo">changeInfo</button>
  <hr />
  <h2>ref 定义响应式变量</h2>
  <div>stateRef： {{ stateRef }}</div>
  <div>sex {{ obj.sex }}</div>
  <div>color {{ newObj.color }}</div>
  <div>color {{ newObj.num }}</div>
  <hr />
  <h2>计算属性</h2>
  <div>{{ comInfo }}</div>
  <hr />
  <h2>监听器 watch</h2>
  <div>ref,numOne:{{ numOne }}, wObj.count: {{ wObj.count }}</div>
  <button @click="decrease">--</button>
  <div>
    <input type="text" ref="inputRef" />
  </div>
</template>

<script setup>
import { isReactive, reactive, ref, computed, watch, onMounted } from "vue";
/**
 * reactive 定义响应式变量
 */
const state = reactive({
  count: 0,
  info: { name: "erXun", age: 18 },
  hobbies: ["ball", "run"],
});

//  reactive，响应式对象的属性赋值，被赋值属性失去响应式
let newCountOne = state.count;
console.log(isReactive(newCountOne), "------------"); // false

// reactive，响应式对象解构，被赋值属性失去响应式
const { count } = state;
let newCountTwo = count;
console.log(isReactive(newCountTwo), "------------"); // false

function increase() {
  state.count++;
}
function changeInfo() {
  state.info.name = "贰旬";
  state.hobbies[2] = "read";
}

/**
 * ref 定义响应式变量
 */
const stateRef = ref(0);
console.log("ref 定义响应式变量", stateRef);

const obj = ref({
  sex: "male",
});
console.log("male", obj.value.sex);

const newObj = {
  color: ref("red"),
  num: ref(100),
};
console.log("newObj.color", newObj.color);

/**
 * 计算属性
 */
const comInfo = computed(() => {
  return obj.value.sex + "---" + newObj.color.value;
});
console.log("计算属性,comInfo", comInfo);

/**
 * watch
 */
let numOne = ref(12);
let numTwo = ref(0);
const wObj = reactive({
  count: 1,
});
watch([numOne, numTwo], (newVal, oldVal) => {
  console.log("val", newVal, oldVal);
});
watch(wObj, (val) => {
  console.log("wObj----------", val);
});
function decrease() {
  numOne.value--;
  wObj.count--;
}

const inputRef = ref(null);
console.log("inputRef", inputRef);
onMounted(() => {
  inputRef.value.focus();
  console.log('inputRef.value', inputRef.value)
});
</script>

<style scoped></style>
