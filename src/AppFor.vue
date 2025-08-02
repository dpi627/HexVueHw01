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