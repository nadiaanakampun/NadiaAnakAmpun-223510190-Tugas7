<template>
  <div id="app">
    <h1>Todo List</h1>
    <input v-model="newTodo" @keyup.enter="addNewTodo" placeholder="Add a new todo" />
    <button @click="addNewTodo">Add</button>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span :class="{ 'todo-completed': todo.done }">{{ todo.text }}</span>
        <button @click="toggleTodoStatus(index)">
          {{ todo.done ? 'Undo' : 'Complete' }}
        </button>
        <button @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
    <p>Unfinished tasks: {{ unfinishedTodosCount }}</p>
  </div>
</template>

<script>
import { ref } from 'vue';
import { useTodoStore } from './stores/todo';

export default {
  setup() {
    const newTodo = ref('');
    const todoStore = useTodoStore();

    const addNewTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value);
        newTodo.value = '';
      }
    };

    return {
      newTodo,
      addNewTodo,
      todos: todoStore.$state.todos,
      removeTodo: todoStore.removeTodo,
      toggleTodoStatus: todoStore.toggleTodoStatus,
      unfinishedTodosCount: todoStore.unfinishedTodosCount,
    };
  },
};
</script>
