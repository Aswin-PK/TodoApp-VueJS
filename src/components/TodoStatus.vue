<template>
    <div class="status-container">
        <div class="headings">
            <span>Todo Done</span>
            <span>Keep it up</span>
        </div>
        <div class="status">
            {{ completedTodo }}/{{ todos.length }}
        </div>
    </div>
</template>


<script>
export default {
    name: 'TodoStatus',
    props: {
        todos: Array,
    },
    computed: {
        completedTodo() {
            return this.todos.filter(todo => todo.isTodoCompleted).length
        }
    }

}
</script>


<style scoped>
.status-container {
    height: 8rem;
    width: 40%;
    border-radius: 1em;
    border: 1px solid #d8c8a8;
    padding: 0 2em;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.status-container .headings {
    display: flex;
    flex-direction: column;
    color: #d8c8a8;
}
.status-container .headings span:first-child {
    font-size: 1.5em;
    font-weight: bold;
}
.status-container .status {
    height: 3em;
    aspect-ratio: 1;
    font-size: 2em;
    font-weight: bold;
    border-radius: 50%;
    background-color: orangered;
    display: grid;
    place-content: center;
}

@media screen and (max-width: 768px) {
    .status-container {
        width: 20em;
    }
}
</style>