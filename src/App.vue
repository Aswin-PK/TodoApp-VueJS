<template>
  <div id="app">
    <TodoStatus :todos="todos"></TodoStatus>
    <TodoForm @add="addTodo"></TodoForm>
    <TodoList :todos="todos" @toggleStatus="updateCompletionStatus" @delete="deleteTodo"></TodoList>
  </div>
</template>

<script>

import TodoStatus from './components/TodoStatus.vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoStatus,
    TodoForm,
    TodoList
  },
  data() {
    return {
      todos: [],
    }
  },
  methods: {

    addTodo(newTask) {
      const newTodo = {
        id: Date.now(),
        task: newTask,
        isCompleted: false
      }

      this.todos.push(newTodo);
    },

    // Update the status of completion of the task
    // recieve ID of the todo from the <TodoItem> component
    updateCompletionStatus(todoId) {

      // finding the index of the todo we need to change status, matching the id it contains
      const todoIndex = this.todos.findIndex(todo => todo.id === todoId);
      if (todoIndex !== -1) {
        this.todos[todoIndex].isCompleted = !this.todos[todoIndex].isCompleted;
      }
    },

    // recieve ID of the todo from the <TodoItem> component
    deleteTodo(todoId) {
      this.todos = this.todos.filter(todo => todo.id !== todoId);
    }
  }
}
</script>

<style scoped>

  #app {
    width: 25rem;
    min-height: 40svh;
    /* background-color: blue; */
    padding: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
</style>
