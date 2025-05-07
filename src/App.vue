<script setup>
import { computed, ref } from 'vue'

const todos = ref([
  { text: 'Cardio', done: false },
  { text: 'Watch Netflix Movie', done: true },
])

const newTodo = ref('')
const filter = ref('all')
const showPopup = ref(false)

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false})
    newTodo.value = ''
    showPopup.value = false
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
  <div class="app">
    <h1 class="title">To-Do List</h1>

    <button class="add-btn" @click="showPopup = true">+</button>

    <div v-if="showPopup" class="popup">
      <div class="popup-card"> 
        <input v-model="newTodo" placeholder="Enter new task" />
        <div class="popup-actions">
          <button @click="addTodo">Add</button>
          <button @click="showPopup = false">Cancel</button>
        </div>
      </div>
    </div>

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
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.app {
  position: relative;
  font-family: 'Press Start 2P', cursive;
  background: #d2ebc7;
  padding: 2rem;
  height: 750px;
  width: 750px;
  border-radius: 20px;
  color: #000;
  box-shadow: 4px 4px #000;
  flex-direction: column;
}

.title {
  text-align: center;
}

h1 {
  font-size: 3.2em;
  line-height: 1.1;
}

.add-btn {
  background: #888;
  color: #fcd34d;
  font-size: 1.2rem;
  border: 2px solid #000;
  padding: 1rem;
  border-radius: 20px;
  position: absolute;
  bottom: 2rem;
  right: 2rem;
  cursor: pointer;
  box-shadow: 2px 2px #000;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0,5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-card {
  background: #fff;
  padding: 2rem;
  border: 2px solid #000;
}

.popup-card input {
  padding: 0.5rem;
  font-family: inherit;
  border: 2px solid #000;
}

.done {
  text-decoration: line-through;
  color: #000;
}
</style>
