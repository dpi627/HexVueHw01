<template>
  <!-- 商品列表區塊 -->
  <div class="container mt-3">
    <h5 class="mb-2">商品列表</h5>
    <div class="table-responsive rounded-3">
    <table class="table table-striped table-hover align-middle">
      <thead class="table-dark">
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
            <input v-else v-model="editingName" class="form-control" />
          </td>
          <td>
            <span v-if="editingItem !== item">
              <small>{{ item.description }}</small>
            </span>
            <input v-else v-model="editingDescription" class="form-control" />
          </td>
          <td>
            <span v-if="editingItem !== item">
              {{ item.price }} 元
            </span>
            <input v-else v-model.number="editingPrice" class="form-control" type="number" />
          </td>
          <td>
            <button @click="decreaseStock(item)" class="btn btn-sm btn-primary btn-font">-</button>
            {{ formatNumber(item.stock) }}
            <button @click="increaseStock(item)" class="btn btn-sm btn-primary btn-font">+</button>
          </td>
          <td>
            <button v-if="editingItem !== item" @click="startEdit(item)" class="btn btn-warning me-2">編輯</button>
            <button v-if="editingItem !== item" @click="deleteItem(item)" class="btn btn-outline-danger">刪除</button>
            <div v-else>
              <button @click="confirmEdit()" class="btn btn-success me-2">確認</button>
              <button @click="cancelEdit()" class="btn btn-outline-secondary">取消</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    </div>

    <!-- 新增區塊 -->
    <h5 class="mb-2 mt-3">新增商品</h5>
    <div class="table-responsive rounded-3">
    <table class="table table-striped table-hover">
      <thead class="table-dark">
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
            <input v-model="newItem.name" class="form-control" placeholder="品項名稱" />
          </td>
          <td>
            <input v-model="newItem.description" class="form-control" placeholder="商品描述" />
          </td>
          <td>
            <!-- .number 修飾符（modifier）將輸入的字串轉換為數字類型 -->
            <input 
              v-model.number="newItem.price" 
              class="form-control text-end" 
              type="number" 
              placeholder="價格" 
              min="0"
            />
          </td>
          <td>
            <input 
              v-model.number="newItem.stock" 
              class="form-control text-end" 
              type="number" 
              placeholder="庫存" 
              min="0"
            />
          </td>
          <td>
            <button class="btn btn-success me-2" @click="addItem">新增</button>
            <button class="btn btn-outline-secondary" @click="clearForm">清空</button>
          </td>
        </tr>
      </tbody>
    </table>
    </div>
  </div>
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
    alert('品項名稱或描述不能為空');
    return;
  }

  if (editingPrice.value > 0) {
    editingItem.value.price = editingPrice.value;
  } else {
    alert('價格必須大於 0');
    return;
  }
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

.btn-font {
  font-size: 0.9rem;
  font-family: Consolas;
}
</style>