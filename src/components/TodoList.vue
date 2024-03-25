<template>
    <div class="todo-container">
        <ul>Not Started Tasks
            <li v-for="(todo) in filteredTodos('Not Started')" :key="todo.id">
                <TodoItem :todo="todo" @statusCompleted="changeStatusCompleted" @editTask="editTask" @delete="deleteTodo" @swap="swapTodo" @moveTask="moveTodo" />
            </li>
            <span v-if="todos.length === 0" style="margin-top: 2rem;">No Task</span>
        </ul>
        <ul class="inprogress-task">In Progress Tasks
            <li v-for="(todo) in filteredTodos('In Progress')" :key="todo.id">
                <TodoItem :todo="todo" @statusCompleted="changeStatusCompleted" @editTask="editTask" @delete="deleteTodo" @swap="swapTodo" @moveTask="moveTodo" />
            </li>
            <span v-if="todos.length === 0" style="margin-top: 2rem;">No Task</span>
        </ul>
        <ul class="completed-task">Completed Tasks
            <li v-for="(todo) in filteredTodos('Completed')" :key="todo.id">
                <TodoItem :todo="todo" @statusCompleted="changeStatusCompleted" @editTask="editTask" @delete="deleteTodo" @swap="swapTodo" @moveTask="moveTodo" />
            </li>
            <span v-if="todos.length === 0" style="margin-top: 2rem;">No Task</span>
        </ul>
    </div>
</template>

<script>

import TodoItem from './TodoItem.vue';

export default {
    name: 'TodoList',
    components: {
        TodoItem
    },
    props: {
        todos: Array,
    },
    methods: {
        filteredTodos(todoStatus) {
            return this.todos.filter(todo => todo.currentTodoStatus === todoStatus)
        },
        changeStatusCompleted(todoId) {
            // toggling the status of completion of Task
            this.$emit('changeStatusCompleted', todoId)
        },
        editTask(todoId) {
            this.$emit('edit', todoId)
        },
        deleteTodo(todoId) {
            this.$emit('delete', todoId);
        },
        swapTodo(todoId) {
            this.$emit('swapTask', todoId)
        },
        moveTodo(direction, currentStatus, todoId) {
            console.log('direction', direction, 'currentStatus', currentStatus)
            this.$emit('moveTodo', direction, currentStatus, todoId)
        }
    }

};
</script>


<style scoped>
    .todo-container {
        margin-top: 1rem;
        width: 100%;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1em
    }
    ul {
        width: 100%;
        min-height: 10em;
        list-style-type: none;
        padding: 0.5em;
        display: flex;
        align-items: center;
        flex-direction: column;
        gap: 0.7rem;
        color: #d8c8a8;
    }
    ul li {
        display: flex;
        align-items: center;
        width: 100%;
    }
    /* ul:not(:last-child) {
        border-right: 2px solid #d8c8a8;
        border-spacing: 2em;
    } */
    ul span {
        color: #b3b3b3d3
    }

    ul.inprogress-task li {
        justify-content: center;
    }
    ul.completed-task li {
        justify-content: end;
    }

    @media screen and (max-width: 768px) {
        .todo-container {
            grid-template-columns: repeat(1, 1fr);
        }
    }

</style>