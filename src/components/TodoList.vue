<template>
    <ul>
        <li v-for="todo in todos" :key="todo.id">
            <TodoItem :todo="todo" @toggleStatus="toggleStatus" @delete="deleteTodo" />
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
        deleteTodo(todoId) {
            this.$emit('delete', todoId);
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