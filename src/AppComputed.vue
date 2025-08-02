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