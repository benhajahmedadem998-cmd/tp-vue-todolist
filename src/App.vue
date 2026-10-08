<script setup>
import { ref, computed } from 'vue'

// 1. Champ de saisie pour le nom d'une tâche
const newTodoText = ref('')

// Tableau réactif contenant la liste des tâches
const todos = ref([])

// 2 & 5. Propriété calculée (computed) pour le nombre de tâches non terminées
const remainingCount = computed(() => {
  return todos.value.filter(todo => !todo.completed).length
})

// 2. Fonction d'ajout d'une tâche
const addTodo = () => {
  const trimmedText = newTodoText.value.trim()
  // Empêcher l'ajout si le champ est vide ou composé uniquement d'espaces
  if (trimmedText === '') return

  todos.value.push({
    id: Date.now(), // Identifiant unique
    text: trimmedText,
    completed: false // État initial non terminée
  })

  // Réinitialiser le champ de saisie
  newTodoText.value = ''
}

// 4. Fonction de suppression d'une tâche
const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<template>
  <div class="app-container">
    <h1>Gestion d'une liste de tâches</h1>

    <!-- 1. Interface : Champ de saisie et bouton Ajouter -->
    <div class="input-container">
      <input 
        v-model="newTodoText" 
        @keyup.enter="addTodo"
        type="text" 
        placeholder="Nom de la tâche..." 
      />
      <button @click="addTodo">Ajouter</button>
    </div>

    <!-- 5. Affichage du compteur / Message si la liste est vide -->
    <div class="counter-section">
      <p v-if="todos.length === 0">Aucune tâche pour le moment.</p>
      <p v-else>Tâches non terminées : {{ remainingCount }}</p>
    </div>

    <!-- 1 & 3. Liste d'affichage des tâches -->
    <ul class="todo-list">
      <li 
        v-for="todo in todos" 
        :key="todo.id" 
        :class="{ completed: todo.completed }"
      >
        <!-- Case à cocher pour modifier l'état de la tâche -->
        <input type="checkbox" v-model="todo.completed" />
        
        <!-- Libellé de la tâche -->
        <span class="todo-text">{{ todo.text }}</span>

        <!-- 4. Bouton Supprimer -->
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

/* 3. Tâche terminée visuellement identifiable (texte barré) */
.completed .todo-text {
  text-decoration: line-through;
  color: #888;
}
</style>