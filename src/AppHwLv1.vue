<template>
  <table>
    <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
        <th scope="col">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in items" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td><small>{{ item.description }}</small></td>
        <td>{{ item.price }}</td>
        <td>
          <button @click="decreaseStock(item)">-</button>
          {{ item.stock }}
          <button @click="increaseStock(item)">+</button>
        </td>
        <td>
          <button>編輯</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import { ref } from 'vue';

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
