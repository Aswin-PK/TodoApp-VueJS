<template>
  <div id="app">
    <TodoStatus :todos="todos"></TodoStatus>
    <TodoForm @handleInput="handleInput" :initialValue="taskToEdit" :initialPriority="priorityToEdit"></TodoForm>
    <TodoList :todos="todos" @changeStatusCompleted="changeStatusCompleted" @edit="editTodo" @delete="deleteTodo"
      @swapTask="handleSwap" @moveTodo="moveTodo"></TodoList>
  </div>
</template>

<script>

import Vue from 'vue';
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
      priorityToEdit: "medium",
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
  computed: {
    insertIndexForMediumPriority() {
      let highIndex = this.todos.findIndex(todo => todo.priority === 'high');

      // if no high priority task is there the index is set to 0 for later calculation to the index for medium-task input as 0
      highIndex = highIndex !== -1 ? highIndex : 0

      let lowIndex = this.todos.findIndex(todo => todo.priority === 'low');

      // if no high priority task is there the index is set to 0 for later calculation to the index for medium-task input as 0
      lowIndex = lowIndex !== -1 ? lowIndex : this.todos.length

      const insertIndex = highIndex + lowIndex;

      return insertIndex
    },

  },
  methods: {

    handleInput(taskInput, selectedPriority) {

      // If the editTodoId is available for the task
      if (this.editTodoId !== null) {

        // fetch index by matching Task Id
        const todoIndex = this.getTodoIndex(this.editTodoId);

        if (todoIndex !== -1) {
          this.todos[todoIndex].task = taskInput
          this.todos[todoIndex].priority = selectedPriority
        }

        this.editTodoId = null

        // To order the todo list in terms of priority after editing
        let todo = this.todos[todoIndex];
        this.todos.splice(todoIndex, 1)
        if (selectedPriority === 'high') this.todos.splice(0, 0, todo)
        else if (selectedPriority === 'low') this.todos.splice(this.todos.length, 0, todo)
        else if (selectedPriority === 'medium') this.todos.splice(this.insertIndexForMediumPriority, 0, todo)

      }
      else {
        const newTodo = {
          id: Date.now(),
          task: taskInput,
          currentTodoStatus: 'Not Started',
          priority: selectedPriority,
          isSwappable: false,
          isTodoCompleted: false
        }

        if (selectedPriority === 'high') this.todos.unshift(newTodo);
        else if (selectedPriority === 'low') this.todos.push(newTodo);
        else this.todos.splice(this.insertIndexForMediumPriority, 0, newTodo);
        // console.log('this.todos', this.todos)
        console.log('Todo Items -- >', this.todos)
      }
    },


    // To find the index of the required task using its ID
    getTodoIndex(todoId) {
      return this.todos.findIndex(todo => todo.id === todoId);
    },

    // Update the status of completion of the task
    // recieve ID of the todo from the <TodoItem> component
    changeStatusCompleted(todoId) {

      // finding the index of the todo we need to change status, matching the id it contains
      const todoIndex = this.getTodoIndex(todoId);
      this.todos[todoIndex].isTodoCompleted = !this.todos[todoIndex].isTodoCompleted;

      if (todoIndex !== -1) {

        this.todos[todoIndex].currentTodoStatus = this.todos[todoIndex].isTodoCompleted ? 'Completed' : 'Not Started';
      }

    },

    // Edit todo
    editTodo(todoId) {

      const todoIndex = this.getTodoIndex(todoId); // fetch index by matching Task Id
      if (todoIndex !== -1) {
        this.editTodoId = todoId
        this.taskToEdit = this.todos[todoIndex].task
        this.priorityToEdit = this.todos[todoIndex].priority
      }
    },


    // Delete Todo by recieving ID of the todo from the <TodoItem> component
    deleteTodo(todoId) {
      this.todos = this.todos.filter(todo => todo.id !== todoId);
    },


    // function to swap todo Items which may or may not be adjacent
    handleSwap(todoId) {

      let todoIndex = this.getTodoIndex(todoId);
      console.log('Index', todoIndex, 'Id', todoId)

      if (this.swapTaskIndex1 === null) this.swapTaskIndex1 = todoIndex;

      else if (this.swapTaskIndex2 === null && todoIndex !== this.swapTaskIndex1) {
        this.swapTaskIndex2 = todoIndex;

        console.log('this.swapTaskIndex1', this.swapTaskIndex1, "Swap Index 2", this.swapTaskIndex2)
        this.interchangeItems();
      }

      else {

        // Reset indices
        this.swapTaskIndex1 = null;
        this.swapTaskIndex2 = null;
      }
    },

    interchangeItems() {

      // Updating the todoItem that is to be swapped
      const swapIndex1_item = { ...this.todos[this.swapTaskIndex1], isTodoCompleted: this.todos[this.swapTaskIndex2].isTodoCompleted, currentTodoStatus: this.todos[this.swapTaskIndex2].currentTodoStatus, isSwappable: false }
      const swapIndex2_item = { ...this.todos[this.swapTaskIndex2], isTodoCompleted: this.todos[this.swapTaskIndex1].isTodoCompleted, currentTodoStatus: this.todos[this.swapTaskIndex1].currentTodoStatus, isSwappable: false }

      Vue.set(this.todos, this.swapTaskIndex1, swapIndex2_item);
      Vue.set(this.todos, this.swapTaskIndex2, swapIndex1_item);

      this.swapTaskIndex1 = null;
      this.swapTaskIndex2 = null;

    },

    moveTodo(direction, currentStatus, todoId) {

      const todoIndex = this.getTodoIndex(todoId);

      // If the right arrow is clicked in the any of the todo List item
      if (direction === 'right') {
        if (currentStatus === 'Not Started') this.todos[todoIndex].currentTodoStatus = 'In Progress'
        else {
          this.todos[todoIndex].currentTodoStatus = 'Completed'
          this.todos[todoIndex].isTodoCompleted = true
        }
      }

      // If the left arrow is clicked in the any of the todo List item
      else {
        if (currentStatus === 'In Progress') this.todos[todoIndex].currentTodoStatus = 'Not Started'
        else {
          this.todos[todoIndex].currentTodoStatus = 'In Progress'
          this.todos[todoIndex].isTodoCompleted = false
        }
      }
    },

  }


}

</script>

<style scoped>
#app {
  width: 90%;
  min-height: 100svh;
  padding: 1.4em 0.5em 0 0.5em;
  display: flex;
  align-items: center;
  /* justify-content: center; */
  flex-direction: column;
}
</style>
