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

## BMI

<details>
    <summary>詳細資料</summary>

```html
<script setup>
import { ref } from 'vue'

// 響應式數據
const height = ref('')
const weight = ref('')
const bmi = ref(0)
const bmiStatus = ref('')
const statusColor = ref('#95a5a6')

// 計算 BMI 的函數
function calculateBMI() {
  if (!height.value || !weight.value) {
    bmi.value = 0
    bmiStatus.value = ''
    statusColor.value = '#95a5a6'
    return
  }
  
  const heightInMeters = height.value / 100
  bmi.value = (weight.value / (heightInMeters * heightInMeters)).toFixed(1)
  
  // 計算狀態
  const bmiValue = parseFloat(bmi.value)
  if (bmiValue < 18.5) {
    bmiStatus.value = '體重過輕'
    statusColor.value = '#3498db'
  } else if (bmiValue < 24) {
    bmiStatus.value = '正常範圍'
    statusColor.value = '#27ae60'
  } else if (bmiValue < 27) {
    bmiStatus.value = '體重過重'
    statusColor.value = '#f39c12'
  } else {
    bmiStatus.value = '肥胖'
    statusColor.value = '#e74c3c'
  }
}
</script>

<template>
  <div class="container">
    <h1>BMI 計算器</h1>
    
    <div class="form-group">
      <label>身高 (公分):</label>
      <!-- v-model 雙向綁定 -->
      <input 
        v-model="height" 
        type="number" 
        placeholder="請輸入身高"
      >
    </div>

    <div class="form-group">
      <label>體重 (公斤):</label>
      <!-- v-model 雙向綁定 -->
      <input 
        v-model="weight" 
        type="number" 
        placeholder="請輸入體重"
      >
    </div>

    <!-- 顯示輸入的數值 -->
    <div class="input-display">
      <p>您輸入的身高: {{ height || '未輸入' }} 公分</p>
      <p>您輸入的體重: {{ weight || '未輸入' }} 公斤</p>
    </div>

    <!-- 計算按鈕 -->
    <div class="button-group">
      <button 
        @click="calculateBMI"
        :disabled="!height || !weight"
      >
        計算 BMI
      </button>
    </div>

    <!-- BMI 結果顯示 -->
    <div class="result" v-if="bmi > 0">
      <h2>您的 BMI 值: {{ bmi }}</h2>
      <!-- v-bind 綁定樣式 -->
      <h3 :style="{ color: statusColor }">{{ bmiStatus }}</h3>
    </div>

    <!-- BMI 參考標準 -->
    <div class="reference">
      <h4>BMI 參考標準:</h4>
      <ul>
        <li :style="{ color: '#3498db' }">過輕: &lt; 18.5</li>
        <li :style="{ color: '#27ae60' }">正常: 18.5 - 23.9</li>
        <li :style="{ color: '#f39c12' }">過重: 24 - 26.9</li>
        <li :style="{ color: '#e74c3c' }">肥胖: ≥ 27</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.input-display {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin: 15px 0;
}

.input-display p {
  margin: 5px 0;
  font-size: 14px;
  color: #666;
}

.button-group {
  text-align: center;
  margin: 20px 0;
}

button {
  padding: 12px 24px;
  font-size: 16px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover:not(:disabled) {
  background-color: #2980b9;
}

button:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
}

.result {
  text-align: center;
  margin: 20px 0;
  padding: 15px;
  background-color: #f0f0f0;
  border-radius: 4px;
}

.reference {
  margin-top: 30px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 4px;
}

.reference h4 {
  margin-top: 0;
  color: #2c3e50;
}

.reference ul {
  list-style: none;
  padding: 0;
}

.reference li {
  padding: 2px 0;
  font-weight: bold;
}
</style>

```
</details>

## Count

<details>
<summary>詳細資料</summary>

```html
<script setup>
import { ref, computed } from 'vue'

// 響應式數據
const userInput = ref('')
const maxLength = ref(50)

// 計算剩餘字數 (使用 computed)
const remainingChars = computed(() => {
  return maxLength.value - userInput.value.length
})

// 清空輸入
const clearInput = () => {
  userInput.value = ''
}
</script>

<template>
  <div class="container">
    <h1>字數倒數計算器</h1>
    
    <!-- 設定字數限制 -->
    <div class="setting-group">
      <label>字數限制:</label>
      <!-- v-model 雙向綁定 -->
      <input 
        v-model="maxLength" 
        type="number" 
        min="10"
        max="200"
      >
    </div>

    <!-- 文字輸入區 -->
    <div class="input-group">
      <label>請輸入文字:</label>
      <!-- v-model 雙向綁定文字內容 -->
      <textarea 
        v-model="userInput"
        placeholder="開始輸入您的文字..."
        rows="4"
      ></textarea>
    </div>

    <!-- 顯示統計資訊 -->
    <div class="info-box">
      <p>已輸入: {{ userInput.length }} 字</p>
      <p>字數限制: {{ maxLength }} 字</p>
      <!-- v-bind 根據剩餘字數改變顏色 -->
      <p :style="{ color: remainingChars >= 0 ? 'green' : 'red' }">
        剩餘: {{ remainingChars }} 字
      </p>
    </div>

    <!-- 即時預覽 -->
    <div class="preview-box">
      <h3>預覽:</h3>
      <div class="preview-text">
        {{ userInput || '您輸入的文字會顯示在這裡...' }}
      </div>
    </div>

    <!-- 操作按鈕 -->
    <div class="button-area">
      <!-- v-bind 控制按鈕是否可點擊 -->
      <button 
        @click="clearInput"
        :disabled="userInput.length === 0"
      >
        清空
      </button>
      
      <button 
        :disabled="remainingChars < 0"
        :style="{ backgroundColor: remainingChars < 0 ? 'gray' : '#007bff' }"
      >
        {{ remainingChars < 0 ? '字數超過!' : '確定' }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 30px auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
}

.setting-group, .input-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

input[type="number"] {
  width: 100px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  resize: vertical;
}

.info-box {
  background-color: #f5f5f5;
  padding: 15px;
  border-radius: 4px;
  margin-bottom: 20px;
}

.info-box p {
  margin: 5px 0;
  font-size: 16px;
}

.preview-box {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 15px;
  margin-bottom: 20px;
}

.preview-box h3 {
  margin-top: 0;
  color: #555;
}

.preview-text {
  min-height: 50px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 4px;
  white-space: pre-wrap;
}

.button-area {
  display: flex;
  gap: 10px;
}

button {
  flex: 1;
  padding: 10px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  color: white;
}

button:first-child {
  background-color: #6c757d;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

button:hover:not(:disabled) {
  opacity: 0.9;
}
</style>
```

</details>

# Homework