<script setup>
import { ref } from 'vue'

const inputValue = ref(1)
const rawHTML = ref('<span style="color: red;">這是一段 HTML</span>')
function showValue() {
  alert(`你輸入的是：${inputValue.value}`)
}
</script>

<template>
  <input v-bind:value="inputValue" type="text" />
  <p>目前輸入：{{ inputValue }}</p>
  <p v-text="inputValue"></p>
  <p v-once>目前輸入：{{ inputValue }} (單向綁定)</p>
  <p v-html="rawHTML"></p>
  
  <button v-on:click="showValue">
    顯示輸入值
  </button>
</template>

<style></style>
