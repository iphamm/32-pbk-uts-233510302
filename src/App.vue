<script setup>
import { ref } from 'vue'

const todos = ref([
  { text: 'Cardio', done: false },
  { text: 'Watch Netflix Movie', done: true },
])

const newTodo = ref('')

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false})
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}
</script>

<template>
  <div>
    <h1>To-Do List</h1>
    <input v-model="newTodo" placeholder="Enter new task" />
    <button @click="addTodo">Add</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(index)">✖</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
  color: #000;
}
</style>
