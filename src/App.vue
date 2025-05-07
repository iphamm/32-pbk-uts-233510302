<script setup>
import { ref } from 'vue'

const todos = ref([
  { text: 'Cardio'},
  { text: 'Watch Netflix Movie'},
])
</script>

<template>
  <div>
    <h1>To-Do List</h1>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        {{ todo.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
