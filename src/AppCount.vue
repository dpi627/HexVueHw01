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

