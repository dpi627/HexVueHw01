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