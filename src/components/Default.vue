<template>
  <div>
    哈哈哈
    <button @click="change">change</button>
    <h2>计算属性 computed</h2>
    姓：<input type="text" v-model="person.fName" />
    <br />
    名：<input type="text" v-model="person.lName" />
    {{ person.fullName }}
  </div>
</template>

<script>
import { computed, reactive } from "@vue/reactivity";
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
    console.log(person);

    function change() {
      context.emit("change", "吼吼吼吼 ");
    }
    return {
      person,
      change,
    };
  },
};
</script>

<style scoped></style>
