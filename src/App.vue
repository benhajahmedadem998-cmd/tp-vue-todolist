<script setup>
import { ref, computed } from 'vue'

const newTodoText = ref('')

const todos = ref([])

const remainingCount = computed(() => {
  return todos.value.filter(todo => !todo.completed).length
})

const addTodo = () => {
  const trimmedText = newTodoText.value.trim()
  if (trimmedText === '') return

  todos.value.push({
    id: Date.now(), 
    text: trimmedText,
    completed: false 
  })

  
  newTodoText.value = ''
}


const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<template>
  <div class="app-container">
    <h1>Gestion d'une liste de tâches</h1>

    
    <div class="input-container">
      <input 
        v-model="newTodoText" 
        @keyup.enter="addTodo"
        type="text" 
        placeholder="Nom de la tâche..." 
      />
      <button @click="addTodo">Ajouter</button>
    </div>

    
    <div class="counter-section">
      <p v-if="todos.length === 0">Aucune tâche pour le moment.</p>
      <p v-else>Tâches non terminées : {{ remainingCount }}</p>
    </div>

    
    <ul class="todo-list">
      <li 
        v-for="todo in todos" 
        :key="todo.id" 
        :class="{ completed: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        
     
        <span class="todo-text">{{ todo.text }}</span>

        <button @click="removeTodo(todo.id)" class="delete-btn">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.app-container {
  max-width: 450px;
  margin: 30px auto;
  font-family: Arial, sans-serif;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.input-container input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
  border: none;
  background-color: #4CAF50;
  color: white;
  border-radius: 4px;
}

.delete-btn {
  background-color: #f44336;
}

.counter-section {
  font-weight: bold;
  margin-bottom: 15px;
  color: #333;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.completed .todo-text {
  text-decoration: line-through;
  color: #888;
}
</style>