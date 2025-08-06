<template>
  <div id="app" class="container">
    <div class="row">
      <div 
        class="col-md-6 mb-3" 
        v-for="(fruit, key) in fruitData" 
        :key="fruit.title"
      >
        <div
          class="card"
            style="width: 100%; position: relative; overflow: hidden;"
        >
          <img
            :src="fruit.imgUrl"
            style="position: absolute; right: 0; bottom: 0; width: 200px; height: auto; opacity: 0.5; filter: blur(4px) opacity(0.2); pointer-events: none; z-index: 0;"
          >
          <div class="card-body d-flex align-items-center">
            <img :src="fruit.imgUrl" style="width: 120px; object-fit: cover; margin-right: 16px;">
            <div>
              <h5 class="card-title">水果名稱：{{ fruit.title }}</h5>
              <p class="card-text my-2">價錢：{{ fruit.price }}</p>
              <p class="card-text mb-3">數量：{{ fruit.count }}</p>
              <a 
                href="#" 
                class="btn btn-primary" 
                @click.prevent="fruit.count > 0 ? fruit.count-- : null"
              >
                加入購物車
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- 請不要修改下面內容 -->
<script setup>
import { ref } from "vue";

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