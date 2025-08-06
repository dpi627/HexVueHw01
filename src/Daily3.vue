<template>
  <div id="app">
    <div class="container">
      <div class="Q1 mb-5">
        <form>
          <div v-for="fruit in fruitData" :key="fruit.title" class="form-check">
            <input 
              type="checkbox" 
              :id="fruit.title" 
              :value="fruit.title" 
              v-model="checkedFruits"
              class="form-check-input"
            />
            <label :for="fruit.title" class="form-check-label">
              <img :src="fruit.imgUrl" height="30px" class="me-2" />
            </label>
          </div>
        </form>
<div class="mt-3">
  透過 checkbox 選擇的水果：
  <span class="d-inline-flex gap-1 ms-2">
    <img 
      v-for="fruitName in checkedFruits" 
      :key="fruitName"
      :src="fruitData.find(fruit => fruit.title === fruitName)?.imgUrl" 
      :alt="fruitName"
      height="30px"
    />
  </span>
</div>
      </div> 

      <div class="Q2">
        <select v-model="selected" class="form-select mb-2">
          <option 
            v-for="fruit in fruitData" 
            :key="fruit.title" 
            :value="fruit.title"
          >
            {{ fruit.title }}
          </option>
        </select>
        <div class="mt-3">
          透過 select 選擇的水果：
          <img 
            :src="fruitData.find(fruit => fruit.title === selected)?.imgUrl" 
            :alt="selected"
            height="30px"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<!-- 請不要修改下面內容 -->
<script setup>
import { ref } from "vue";
  
const checkedFruits = ref([]);
const selected = ref('');
  
const fruitData = ref([
  {
    title: 'apple',
    price: 25,
    count: 50,
    imgUrl: 'https://i.imgur.com/w4sYWlS.jpeg'
  },
  {
    title: 'orange',
    price: 15,
    count: 20,
    imgUrl: 'https://i.imgur.com/PSmzmXi.jpeg'
  },
  {
    title: 'strawberry',
    price: 45,
    count: 10,
    imgUrl: 'https://i.imgur.com/FIMmh6h.png',
  },
  {
    title: 'kiwi',
    price: 55,
    count: 20,
    imgUrl: 'https://i.imgur.com/TIA6v4m.jpeg'
  }
]);
</script>

<style>
  .container {
    max-width: 600px;
    margin-top: 50px;
  }
  .form-check {
    display: flex;
    align-items: center;
  }
  .form-check-label {
    margin-left: 0.5rem;
  }
</style>
