<template>
  <!-- 商品列表區塊 -->
  <h3>商品列表</h3>
  <table>
    <thead>
      <tr>
        <th width="5%" scope="col">#</th>
        <th width="30%" scope="col">品項</th>
        <th width="30%" scope="col">描述</th>
        <th width="10%" scope="col">價格</th>
        <th width="10%" scope="col">庫存</th>
        <th width="15%" scope="col">操作</th>
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
        <td>
          <span v-if="editingItem !== item">
          <small>{{ item.description }}</small>
          </span>
          <input v-else v-model="editingDescription" />
        </td>
        <td>
          <span v-if="editingItem !== item">
            {{ item.price }} 元
          </span>
          <input v-else v-model.number="editingPrice" type="number" />
        </td>
        <td>
          <button @click="decreaseStock(item)">-</button>
          {{ formatNumber(item.stock) }}
          <button @click="increaseStock(item)">+</button>
        </td>
        <td>
          <button v-if="editingItem !== item" @click="startEdit(item)">編輯</button>
          <button v-if="editingItem !== item" @click="deleteItem(item)">刪除</button>
          <div v-else>
            <button @click="confirmEdit()">確認</button>
            <button @click="cancelEdit()">取消</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>

  <!-- 新增區塊 -->
  <h3>新增商品</h3>
  <table>
    <thead>
      <tr>
        <th width="5%" scope="col"></th>
        <th width="30%" scope="col">品項</th>
        <th width="30%" scope="col">描述</th>
        <th width="10%" scope="col">價格</th>
        <th width="10%" scope="col">庫存</th>
        <th width="15%" scope="col">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td></td>
        <td>
          <input v-model="newItem.name" style="width: 90%;" placeholder="品項名稱" />
        </td>
        <td>
          <input v-model="newItem.description" style="width: 90%;" placeholder="商品描述" />
        </td>
        <td>
          <!-- .number 修飾符（modifier）將輸入的字串轉換為數字類型 -->
          <input v-model.number="newItem.price" style="width: 90%;" type="number" placeholder="價格" />
        </td>
        <td>
          <input v-model.number="newItem.stock" style="width: 90%;" type="number" placeholder="庫存" />
        </td>
        <td>
          <button @click="addItem">新增</button>
          <button @click="clearForm">清空</button>
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
const editingDescription = ref('');
const editingPrice = ref(0);

// 新增商品的表單資料
const newItem = ref({
  name: '',
  description: '',
  price: 0,
  stock: 0
});

// 新增商品
const addItem = () => {
  // 驗證必填欄位
  if (!newItem.value.name.trim()) {
    alert('請輸入品項名稱');
    return;
  }
  if (!newItem.value.description.trim()) {
    alert('請輸入商品描述');
    return;
  }
  if (newItem.value.price <= 0) {
    alert('價格必須大於 0');
    return;
  }
  if (newItem.value.stock < 0) {
    alert('庫存不能為負數');
    return;
  }

  // 新增到商品列表
  items.value.push({
    name: newItem.value.name.trim(),
    description: newItem.value.description.trim(),
    price: newItem.value.price,
    stock: newItem.value.stock
  });

  // 清空表單
  clearForm();
};

// 清空表單
const clearForm = () => {
  newItem.value = {
    name: '',
    description: '',
    price: 0,
    stock: 0
  };
};

// 刪除商品
const deleteItem = (item) => {
  const isDelete = confirm(`確定刪除「${item.name}」？`);

  if (isDelete) {
    // 直接過濾掉要刪除的項目
    items.value = items.value.filter(i => i !== item);
  }
};

// 數字格式化函數
const formatNumber = (num) => {
  return num.toString().padStart(2, '0');
};

// 開始編輯
const startEdit = (item) => {
  editingItem.value = item;
  editingName.value = item.name;
  editingDescription.value = item.description;
  editingPrice.value = item.price;
};

// 確認編輯狀態
const confirmEdit = () => {
  if (editingName.value.trim() !== '' &&
      editingDescription.value.trim() !== '') {
    editingItem.value.name = editingName.value.trim();
    editingItem.value.description = editingDescription.value.trim();
  }
  else {
    alert('品項名稱或描述不能為空，請重新操作');
  }

  if (editingPrice.value > 0) {
    editingItem.value.price = editingPrice.value;
  } else {
    alert('價格必須大於 0');
    return;
  }

  cancelEdit();
};

// 取消編輯
const cancelEdit = () => {
  editingItem.value = null;
  editingName.value = '';
  editingDescription.value = '';
  editingPrice.value = 0;
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
  margin: 0 auto;
  /* 讓表格本身置中 */
}

th,
td {
  text-align: center;
  /* 讓所有表格內容置中 */
  padding: 8px;
  border: 1px solid #ddd;
}
</style>