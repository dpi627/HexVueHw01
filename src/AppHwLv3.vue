<template>
  <table>
    <thead>
      <tr>
        <th width="5%" scope="col">#</th>
        <th width="25%" scope="col">品項</th>
        <th width="25%" scope="col">描述</th>
        <th width="10%" scope="col">價格</th>
        <th width="10%" scope="col">庫存</th>
        <th width="10%" scope="col">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in items" :key="index">
        <td>{{ formatNumber(index + 1) }}</td>
        <td>
          <span v-if="editingItem !== item">
            {{ item.name }}
          </span>
          <input v-else v-model="editingName" />
        </td>
        <td><small>{{ item.description }}</small></td>
        <td>{{ item.price }}</td>
        <td>
          <button @click="decreaseStock(item)">-</button>
          {{ formatNumber(item.stock) }}
          <button @click="increaseStock(item)">+</button>
        </td>
        <td>
          <button v-if="editingItem !== item" @click="startEdit(item)">編輯</button>
          <div v-else>
            <button @click="confirmEdit()">確認</button>
            <button @click="cancelEdit()">取消</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref } from 'vue';

// 編輯相關資料狀態
const editingItem = ref(null);
const editingName = ref('');

// 數字格式化函數
const formatNumber = (num) => {
  return num.toString().padStart(2, '0');
};

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

const increaseStock = (item) => { item.stock++ };
const decreaseStock = (item) => { if (item.stock > 0) item.stock-- };

const items = ref([
  { name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, stock: 20 },
  { name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, stock: 18 },
  { name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, stock: 34 },
  { name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, stock: 10 },
  { name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, stock: 25 },
  { name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, stock: 20 },
  { name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, stock: 18 },
  { name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, stock: 20 }
]);
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin: 0 auto; /* 讓表格本身置中 */
}

th, td {
  text-align: center; /* 讓所有表格內容置中 */
  padding: 8px;
  border: 1px solid #ddd;
}
</style>