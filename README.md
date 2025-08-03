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

|    Directive    | Description                     |
| :-------------: | ------------------------------- |
|    `v-text`     | 等同 `{{}}` 但可避免閃爍        |
|    `v-html`     | 渲染原始 HTML，要注意 XSS       |
|    `v-once`     | 渲染靜態內容，單次綁定          |
|    `v-bind`     | 縮寫 `:` 綁定 HTML Attribute    |
|     `v-on`      | 縮寫 `@` addEventListener       |
|     `v-for`     | 迴圈，建議搭配 `:key="item.id"` |
| `v-if` `v-else` | 邏輯判斷                        |

> [!CAUTION]
> `v-html` 可渲染 HTML 標籤，其中也包含了 `<script`，所以要特別注意 XSS（Cross-Site Scripting）攻擊

# In-Class Exercise

> 💡執行 `npm run dev` 之前記得先執行 `npm install`

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

> [!NOTE]
> 執行範例的時候，可建立一個 `AppXXX.vue` 貼上程式，然後修改 `main.js`

## BMI

<details>
    <summary>範例程式</summary>

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

## Literal Count

<details>
<summary>範例程式</summary>

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

## Computed

```html
<script setup>
import { ref, computed } from 'vue';

const lunchPrice = ref(90);
const drinkPrice = ref(65);

// Computed：計算總額
const totalSpent = computed(() => lunchPrice.value + drinkPrice.value);

// Method：計算總額
function getTotalSpent() {
  return lunchPrice.value + drinkPrice.value;
}
</script>

<template>
    <!-- 使用 Computed -->
    <p>消費總額 (Computed): {{ totalSpent }}</p>
    <!-- 使用 Method -->
    <p>消費總額 (Method): {{ getTotalSpent() }}</p>
    <!-- 吃不飽加碼 -->
    <button @click="lunchPrice += 10">吃不飽</button>
</template>
```

| 特性         | Computed                         | Method                                    |
| ------------ | -------------------------------- | ----------------------------------------- |
| **模板**     | 直接使用 `{{ variable }}`        | 需加 `()` `{{ getData() }}`               |
| **快取**     | 有，資料沒變重用結果             | 無，每次都重新執行                        |
| **性能**     | 高，適合多次使用或複雜計算       | 低，多次調用可能導致性能問題              |
| **適用場景** | 衍生資料，如計算餘款、格式化數據 | 適合做「觸發動作」：如 按鈕事件、API 呼叫 |

## v-model

<details>
<summary>範例程式</summary>

```html
<template>
    <h1>待辦事項</h1>
    <table border="1">
      <thead>
        <tr>
          <th>待辦事項</th>
          <th>到期日</th>
          <th>已完成</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todo in todos" :key="todo.name">
          <td v-text="todo.name"></td>
          <td v-html="formatDueDate(todo.dueDate)"></td>
          <td>{{ todo.completed ? '是' : '否' }}</td>
        </tr>
      </tbody>
    </table>

    <h1>新增 (v-model 雙向綁定)</h1>
    <div>
      <input v-model="newTodoName" placeholder="新增待辦事項" />
      <input v-model="newTodoDueDate" type="date" />
      <button @click="addTodo">新增</button>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const todos = ref([
  { name: '購買雜貨', dueDate: '2024-07-30', completed: true },
  { name: '完成報告', dueDate: '2024-08-01', completed: false },
  { name: '清理房間', dueDate: '2024-07-28', completed: true }
]);

const newTodoName = ref('');
const newTodoDueDate = ref('');

// 格式化到期日（模擬 HTML 渲染）
const formatDueDate = (date) => {
  return `<span style="color: #336699;">${date}</span>`;
};

// 新增待辦事項
const addTodo = () => {
  if (newTodoName.value && newTodoDueDate.value) {
    todos.value.push({
      name: newTodoName.value,
      dueDate: newTodoDueDate.value,
      completed: false,
    });
    newTodoName.value = '';
    newTodoDueDate.value = '';
  }
};
</script>
```

</details>

如果以 `MVVM` 比喻

- `todos` 集合的單筆資料結構就是 `Model`
- `<template>` 自然就是 `View`
- `<script setup>` 基本上可類比為 `ViewModel`

# Homework

## 餐點管理

- Level 1：將菜單轉為資料格式
- Level 2：可以重新設定菜單的庫存數量
- Level 3（挑戰）：可以重新設定品項名稱

## 作業需求：

- 將以下的表格轉為使用資料，並使用 Vue 進行渲染。
- 數量的部分可以點擊，並且調整庫存數量。
- 庫存數量不會低於 0。
- 新增欄位 “編輯”，按下後可以修改該欄位的品項名稱（按下確認後執行更換）

<details>
<summary>原始程式碼(未拆版)</summary>

```html
<table>
  <thead>
    <tr>
      <th scope="col">品項</th>
      <th scope="col">描述</th>
      <th scope="col">價格</th>
      <th scope="col">庫存</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>珍珠奶茶</td>
      <td><small>香濃奶茶搭配QQ珍珠</small></td>
      <td>50</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>冬瓜檸檬</td>
      <td><small>清新冬瓜配上新鮮檸檬</small></td>
      <td>45</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>翡翠檸檬</td>
      <td><small>綠茶與檸檬的完美結合</small></td>
      <td>55</td>
      <td><button>-</button>34<button>+</button></td>
    </tr>
    <tr>
      <td>四季春茶</td>
      <td><small>香醇四季春茶，回甘無比</small></td>
      <td>45</td>
      <td><button>-</button>10<button>+</button></td>
    </tr>
    <tr>
      <td>阿薩姆奶茶</td>
      <td><small>阿薩姆紅茶搭配香醇鮮奶</small></td>
      <td>50</td>
      <td><button>-</button>25<button>+</button></td>
    </tr>
    <tr>
      <td>檸檬冰茶</td>
      <td><small>檸檬與冰茶的清新組合</small></td>
      <td>45</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>芒果綠茶</td>
      <td><small>芒果與綠茶的獨特風味</small></td>
      <td>55</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>抹茶拿鐵</td>
      <td><small>抹茶與鮮奶的絕配</small></td>
      <td>60</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
  </tbody>
</table>
```
</details>

## 實作

<details>
<summary>Level 1 菜單轉為資料格式</summary>

```html
<template>
  <table>
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in items" :key="item.id">
        <td>{{ item.id }}</td>
        <td>{{ item.name}}</td>
        <td><small>{{ item.description}}</small></td>
        <td>{{ item.price}}</td>
        <td><button>-</button>{{ item.stock }}<button>+</button></td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref } from 'vue';

const items = ref([
  { id: 1, name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, stock: 20 },
  { id: 2, name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, stock: 18 },
  { id: 3, name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, stock: 34 },
  { id: 4, name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, stock: 10 },
  { id: 5, name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, stock: 25 },
  { id: 6, name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, stock: 20 },
  { id: 7, name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, stock: 18 },
  { id: 8, name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, stock: 20 }
]);
</script>
```

</details>

<details>
<summary>Level 2 可編輯庫存 (庫存不低於零)</summary>

```html
<template>
  <!-- skip code -->
  <td>
    <button @click="decreaseStock(item)">-</button>
    {{ item.stock }}
    <button @click="increaseStock(item)">+</button>
  </td>
  <!-- skip code -->
</template>

<script setup>
  // 增加庫存
  const increaseStock = (item) => { item.stock++ };
  // 減少庫存，不低於零
  const decreaseStock = (item) => { if (item.stock > 0) item.stock-- };
</script>
```

</details>

<details>
<summary>Level 3 新增欄位 "編輯"，按下 "確認" 修改名稱</summary>

```html
<template>
  <!-- skip code -->
  <td>
    <span v-if="editingItem !== item">
      {{ item.name }}
    </span>
    <input v-else v-model="editingName" />
  </td>
  <!-- skip code -->
  <td>
    <button v-if="editingItem !== item" @click="startEdit(item)">編輯</button>
    <div v-else>
      <button @click="confirmEdit()">確認</button>
      <button @click="cancelEdit()">取消</button>
    </div>
  </td>
  <!-- skip code -->
</template>

<script setup>
// 編輯相關資料狀態
const editingItem = ref(null);
const editingName = ref('');

// 開始編輯
const startEdit = (item) => {
  editingItem.value = item;
  editingName.value = item.name;
};

// 確認編輯狀態
const confirmEdit = () => {
  if (editingName.value.trim() !== '') {
    editingItem.value.name = editingName.value.trim();
  }
  else {
    alert('品項名稱不能為空，請重新操作');
  }
  cancelEdit();
};

// 取消編輯
const cancelEdit = () => {
  editingItem.value = null;
  editingName.value = '';
};
</script>
```

</details>

### Additional Features

- Add new item
- Delete item
- Integrate Bootstrap
- Minimal layout design