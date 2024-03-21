<template>
  <div id="app">
    <TodoStatus :todos="todos"></TodoStatus>
    <TodoForm @handleInput="handleInput" :initialValue="taskToEdit"></TodoForm>
    <TodoList :todos="sortedTaskList" @toggleStatus="updateCompletionStatus" @edit="editTodo" @delete="deleteTodo" @swapTask="handleTodoSwap"></TodoList>
  </div>
</template>

<script>

// import Vue from 'vue';
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

      // console.log('sortedTasks == ', sortedTasks)
      return sortedTasks
    }
  },
  methods: {

    handleInput(taskInput, selectedPriority) {

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
    },


    // Delete Todo by recieving ID of the todo from the <TodoItem> component
    deleteTodo(todoId) {
      this.todos = this.todos.filter(todo => todo.id !== todoId);
    },

 
    handleTodoSwap(direction, todoIndex){
      
      if (direction === 'down' && todoIndex < this.sortedTaskList.length - 1) {
        
        // Storing the taskname and status of the list to be swapped

        const swappingTaskname = this.sortedTaskList[todoIndex].task
        const swappingTaskStatus = this.sortedTaskList[todoIndex].isCompleted
        const updatedTodos = [...this.sortedTaskList]

        // Updating the todoItem that is to be swapped

        updatedTodos[todoIndex] = {
          ...updatedTodos[todoIndex],
          task: this.sortedTaskList[todoIndex+1].task,
          isCompleted: this.sortedTaskList[todoIndex+1].isCompleted
        }
        
        // Updating the todoItem that is to be swapped with

        updatedTodos[todoIndex + 1] = {
          ...updatedTodos[todoIndex + 1],
          task: swappingTaskname,
          isCompleted: swappingTaskStatus
        }

        this.todos = updatedTodos

      }

      else if(direction === 'up' && todoIndex > 0) {

        // Storing the taskname and status of the list to be swapped
        const swappingTaskname = this.sortedTaskList[todoIndex].task
        const swappingTaskStatus = this.sortedTaskList[todoIndex].isCompleted

        const updatedTodos = [...this.sortedTaskList]
        
        // Updating the todoItem that is to be swapped
        updatedTodos[todoIndex] = {
          ...updatedTodos[todoIndex],
          task: this.sortedTaskList[todoIndex - 1].task,
          isCompleted: this.sortedTaskList[todoIndex - 1].isCompleted
        }

        // Updating the todoItem that is to be swapped with
        updatedTodos[todoIndex - 1] = {
          ...updatedTodos[todoIndex - 1],
          task: swappingTaskname,
          isCompleted: swappingTaskStatus
        }

        this.todos = updatedTodos   
      
      }
    }


  }
}
</script>

<style scoped>

  #app {
    width: 25rem;
    min-height: 40svh;
    padding: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
</style>
