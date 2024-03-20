<template>
  <div id="app">
    <TodoStatus :todos="todos"></TodoStatus>
    <TodoForm @handleInput="handleInput" :initialValue="taskToEdit"></TodoForm>
    <TodoList :todos="sortedTaskList" @toggleStatus="updateCompletionStatus" @edit="editTodo" @delete="deleteTodo"></TodoList>
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
      taskToEdit: "",
      editTodoId: null
    }
  },
  computed: {
    sortedTaskList() {
      const priorityAllocation = {
        low: 0,
        medium: 1,
        high: 2
      }

      const sortedTasks = [...this.todos].sort((a, b) => priorityAllocation[b.priority] - priorityAllocation[a.priority]);

      console.log('sortedTasks - ', sortedTasks)
      return sortedTasks
    }
  },
  methods: {

    handleInput(taskInput, selectedPriority) {

      console.log('taskInput -> ', taskInput)

      if(this.editTodoId !== null) {
        const todoIndex = this.todos.findIndex(todo => todo.id === this.editTodoId); // fetch index by matching Task Id
        
        if(todoIndex !== -1) {
          this.todos[todoIndex].task = taskInput 
          this.todos[todoIndex].priority = selectedPriority
        }
        this.editTodoId = null
      }
      else {
        const newTodo = {
          id: Date.now(),
          task: taskInput,
          isCompleted: false,
          priority: selectedPriority,
        }
        
        this.todos.push(newTodo);
      }
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

    // Edit todo
    editTodo(todoId) {

      const todoIndex = this.todos.findIndex(todo => todo.id === todoId); // fetch index by matching Task Id
      if (todoIndex !== -1) {
        this.editTodoId = todoId
        this.taskToEdit = this.todos[todoIndex].task
      }

      // if(editedTask !== null) console.log('first -> ', editedTask)

      //   if(editedTask !== undefined) {
      //     this.todos[todoIndex].task = editedTask
      //   }

      //   else {
      //     this.taskToEdit = this.todos[todoIndex];
      //   }
      // }

    },


    // Delete Todo by recieving ID of the todo from the <TodoItem> component
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
