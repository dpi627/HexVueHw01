# HEX Vue3 Bootcamp Week 1

https://hackmd.io/o-BW3WhjRWOJjCUfqJ4YJg

# Quick Note

- 推薦使用 Vite 建立專案
- `*.vue` 包含三部分 `<script>` `<template>` `<style>`
- 上述三個部分沒有順序要求，一定要存在 `<template>`
- 官方建議存資料一律使用 `ref`，記得取值要加上 `.value`

## `*.vue` 結構

```xml
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

# In-Class Exercise

# Homework