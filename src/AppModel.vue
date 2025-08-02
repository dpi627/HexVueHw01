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