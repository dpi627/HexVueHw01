# HEX Vue3 Bootcamp Week 1

https://hackmd.io/o-BW3WhjRWOJjCUfqJ4YJg

# Quick Note

- 推薦使用 Vite 建立專案
- `*.vue` 包含三部分 `<script>` `<template>` `<style>`
- 上述三個部分沒有順序要求，一定要存在 `<template>`
- 官方建議存資料一律使用 `ref`，記得取值要加上 `.value`

## `*.vue` 結構

```html
<script setup>...</script>

<template>...</template>

<style>...</style>
```

## 常用指令

|Directive|Description|
|:-:|-|
|`v-text`|等同 `{{}}` 但可避免閃爍|
|`v-html`|渲染原始 HTML，要注意 XSS|
|`v-once`|渲染靜態內容，單次綁定|
|`v-bind`|綁定 HTML Attribute|
|`v-for`|迴圈|
|`v-on`|addEventListener|
|`v-if` `v-else`|邏輯判斷|

> [!CAUTION]
> `v-html` 可渲染 HTML 標籤，其中也包含了 `<script`，所以要特別注意 XSS（Cross-Site Scripting）攻擊

# In-Class Exercise

> 執行 `npm run dev` 之前記得先執行 `npm install`

## Quick Demo

```html
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
```

## v-for

```html
<script setup>
import { ref } from 'vue'

const students = ref([
  { id: 'S01', name: '小明', score: 90 },
  { id: 'S02', name: '小花', score: 85 },
  { id: 'S03', name: '阿狗', score: 78 }
])
</script>

<template>
    <h2>學生名單</h2>
    <ul>
      <li v-for="student in students" :key="student.id">
        <p>學號：{{ student.id }}</p>
        <p>姓名：{{ student.name }}</p>
        <p>分數：{{ student.score }}</p>
      </li>
    </ul>
</template>
```

# Homework