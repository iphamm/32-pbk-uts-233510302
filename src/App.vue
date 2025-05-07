<script setup>
import { computed, ref } from 'vue'

const todos = ref([
  { text: 'Cardio', done: false },
  { text: 'Watch Netflix Movie', done: true },
])

const newTodo = ref('')
const filter = ref('all')

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false})
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'todo') return todos.value.filter(t => !t.done)
  if (filter.value === 'done') return todos.value.filter(t => t.done)
  return todos.value
})
</script>

<template>
  <div>
    <h1>To-Do List</h1>
    <input v-model="newTodo" placeholder="Enter new task" />
    <button @click="addTodo">Add</button>
    <button @click="filter = 'all'" :class="{ active: filter === 'all' }">All</button>
    <button @click="filter = 'todo'" :class="{ active: filter === 'todo' }">To Do</button>
    <button @click="filter = 'done'" :class="{ active: filter === 'done' }">Done</button>
    <ul>
      <li v-for="(todo, index) in filteredTodos" :key="index">
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
