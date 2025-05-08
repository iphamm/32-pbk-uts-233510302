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
          <button class="add-btn-popup" @click="addTodo">Add</button>
          <button class="cancel-btn-popup" @click="showPopup = false">Cancel</button>
        </div>
      </div>
    </div>

    <div class="navbar">
      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">All</button>
      <button @click="filter = 'todo'" :class="{ active: filter === 'todo' }">To Do</button>
      <button @click="filter = 'done'" :class="{ active: filter === 'done' }">Done</button>
    </div>

    <div class="todo-list">
      <div v-for="(todo, index) in filteredTodos" :key="index" class="todo-item">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button class="cancel" @click="removeTodo(index)">✖</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.app {
  position: relative;
  font-family: 'Press Start 2P', cursive;
  background: #c6e0bb;
  padding: 2rem;
  height: 750px;
  width: 750px;
  color: #000;
  box-shadow: 4px 4px #000;
  display: flex;
  flex-direction: column;
}

.title {
  font-size: 3.2em;
  line-height: 1.1;
  text-align: center;
}

.add-btn {
  background: #7cc74c  ;
  color: #000;
  font-size: 1.4rem;
  font-family: 'Press Start 2P', cursive;
  border: 2px solid #000;
  padding: 1.4rem 1.4rem;
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
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.popup-card {
  background: #fcd34d;
  padding: 2rem;
  border: 2px solid #000;
  box-shadow: 2px 2px #000;
}

.popup-card input {
  background-color: #fefae0;
  padding: 0.5rem;
  margin-bottom: 1rem;
  font-family: 'Press Start 2P', cursive;
  border: 2px solid #000;
}

.popup-actions {
  display: flex;
  justify-content: space-between;
}

.add-btn-popup {
  background-color: #7cc74c; 
  color: #000;
  font-family: 'Press Start 2P', cursive;
  font-size: 0.75rem;
  border: 2px solid #000;
  padding: 1rem 1.5rem;
  box-shadow: 2px 2px #000;
  cursor: pointer;
}

.add-btn-popup:active {
  background-color: #5aab2b; 
  box-shadow: 1px 1px #000;
  transform: translate(2px, 2px);
}

.cancel-btn-popup {
  background-color: #f87171; 
  color: #000;
  font-family: 'Press Start 2P', cursive;
  font-size: 0.75rem;
  border: 2px solid #000;
  padding: 1rem 1.5rem;
  box-shadow: 2px 2px #000;
  cursor: pointer;
}

.cancel-btn-popup:active {
  background-color: #c53030; 
  box-shadow: 1px 1px #000;
  transform: translate(2px, 2px);
}

.navbar {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.navbar button {
  font-family: inherit;
  background: #fcd34d;
  border: 2px solid #000;
  padding: 1rem 2rem;
  font-size: 1.2rem;
  font-weight: bold;
  color: #000;
  cursor: pointer;
  box-shadow: 2px 2px #000;
}

.navbar .active {
  background: #f87171;
  color: #000;
}

.todo-list {
  max-height: 250px;
  overflow-y: auto;
  margin-bottom: 1rem;
}

.todo-list::-webkit-scrollbar {
  width: 8px;
}

.todo-list::-webkit-scrollbar-thumb {
  background: #7cc74c;
  border: 2px solid #000;
}

.todo-item {
  background: #fefae0;
  border: 2px solid #000;
  margin-bottom: 0.5rem;
  padding: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  box-shadow: 2px 2px #000;
}

.todo-item span {
  flex-grow: 1;
  word-break: break-word;
  text-align: center;
}

.todo-item input[type="checkbox"] {
  position: relative;
  appearance: none;
  width: 20px;
  height: 20px;
  min-width: 20px;
  min-height: 20px;
  background-color: #fff;
  border: 2px solid #000;
  cursor: pointer;
  box-shadow: 2px 2px #000;
}

.todo-item input[type="checkbox"]:checked {
  background-color: #7cc74c; 
}

.todo-item input[type="checkbox"]::after {
  content: '';
  position: absolute;
  top: 3px;
  left: 5px;
  width: 6px;
  height: 10px;
  border: solid #000;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
  display: none;
}

.todo-item input[type="checkbox"]:checked::after {
  display: block;
}

.todo-item input[type="checkbox"]:hover {
  border-color: #ccc0c0;
}

.todo-item .done {
  text-decoration: line-through;
  color: #888;
}

.cancel {
  width: 20px;
  height: 20px;
  min-width: 20px; 
  min-height: 20px;
  background: #f87171;
  border: 2px solid #000;
  color: #000;
  font-size: 14px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 2px 2px #000;
  cursor: pointer;
}

.cancel:hover {
  background: #ef4444;
  box-shadow: 1px 1px #000;
  transform: translate(1px, 1px);
}

button {
  transition: border-color 0.25s;
}

button:hover {
  border-color: #ffffff
}
</style>
