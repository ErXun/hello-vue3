<template>
  <!-- <Study /> -->
  <Default msg="哈哈哈" school="炳辉" @change="change"></Default>
</template>

<script setup>
import Default from "./components/Default.vue";
// import Study from "./components/Study.vue";

const change = (val) => {
  console.log("val", val);
};
</script>

<style></style>
