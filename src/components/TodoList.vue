<template>
    <ul>
        <li v-for="(todo, index) in todos" :key="todo.id">
            <TodoItem :todo="todo" :index="index" @toggleStatus="toggleStatus" @editTask="editTask" @delete="deleteTodo" @swap="swapTodo" />
        </li>
    </ul>
</template>

<script>

import TodoItem from './TodoItem.vue';

export default {
    name: 'TodoList',
    components: {
        TodoItem
    },
    props: {
        todos: Array
    },
    methods: {
        toggleStatus(todoId) {
            // toggling the status of completion of Task
            this.$emit('toggleStatus', todoId)
        },
        editTask(todoId) {
            this.$emit('edit', todoId)
        },
        deleteTodo(todoId) {
            this.$emit('delete', todoId);
        },
        swapTodo(swapIndex) {
            this.$emit('swapTask', swapIndex)
        }
    }

};
</script>


<style scoped>
    ul {
        width: 100%;
        list-style-type: none;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        gap: 0.7rem;
    }
    ul li {
        width: 100%;
    }

</style>