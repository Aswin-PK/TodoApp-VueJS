<template>
  <div id="app">
    <TodoStatus :todos="todos"></TodoStatus>
    <TodoForm @handleInput="handleInput" :initialValue="taskToEdit"></TodoForm>
    <TodoList :todos="todos" @toggleStatus="updateCompletionStatus" @edit="editTodo" @delete="deleteTodo"
      @swapTask="handleSwap"></TodoList>
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
      editTodoId: null,
      swapTaskIndex1: null,
      swapTaskIndex2: null,
      priorityAllocation: {
        low: 0,
        medium: 1,
        high: 2
      }
    }
  },
  methods: {

    handleInput(taskInput, selectedPriority) {

      // If the editTodoId is available for the task
      if (this.editTodoId !== null) {

        // fetch index by matching Task Id
        const todoIndex = this.todos.findIndex(todo => todo.id === this.editTodoId); 

        if (todoIndex !== -1) {
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
          isSwappable: false
        }

        if(selectedPriority === 'high') this.todos.unshift(newTodo);
        else if(selectedPriority === 'low') this.todos.push(newTodo);
        else {
          let highIndex = this.todos.findIndex(todo => todo.priority === 'high');

          // if no high priority task is there the index is set to 0 for later calculation to the index for medium-task input as 0
          highIndex = highIndex !== -1 ? highIndex : 0 

          const lowIndex = this.todos.findIndex(todo => todo.priority === 'low');

          const insertIndex = highIndex + lowIndex; // to get the index where medium priority todo need to be inserted

          this.todos.splice(insertIndex, 0, newTodo);
        }
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


    // function to swap todo Items which may or may not be adjacent
    handleSwap(todoIndex) {

      if (this.swapTaskIndex1 === null) this.swapTaskIndex1 = todoIndex;

      else if (this.swapTaskIndex2 === null && todoIndex !== this.swapTaskIndex1) {
        this.swapTaskIndex2 = todoIndex;
        this.interchangeItems();
      } 
      
      else {

        // Reset indices
        this.swapTaskIndex1 = null;
        this.swapTaskIndex2 = null;
      }
    },

    interchangeItems() {

      const updatedTodos = [...this.todos]

      // Updating the todoItem that is to be swapped
      updatedTodos[this.swapTaskIndex1] = {
        ...updatedTodos[this.swapTaskIndex1],
        task: this.todos[this.swapTaskIndex2].task,
        priority: this.todos[this.swapTaskIndex2].priority,
        isCompleted: this.todos[this.swapTaskIndex2].isCompleted,
        isSwappable: false
      }

      // Updating the todoItem that is to be swapped with
      updatedTodos[this.swapTaskIndex2] = {
        ...updatedTodos[this.swapTaskIndex2],
        task: this.todos[this.swapTaskIndex1].task,
        priority: this.todos[this.swapTaskIndex1].priority,
        isCompleted: this.todos[this.swapTaskIndex1].isCompleted,
        isSwappable: false
      }

      this.todos = updatedTodos

      this.swapTaskIndex1 = null;
      this.swapTaskIndex2 = null;
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
