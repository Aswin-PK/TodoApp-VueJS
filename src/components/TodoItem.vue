<template>
    <div class="todo-item">
        <div class="left-side">
            <span @click="toggleStatus" id="task-status" :class="{ taskcompleted: todo.isCompleted }"></span>
            <span :class="{ completed: todo.isCompleted }">{{ todo.task }}</span>
        </div>
        <div class="right-side">
            <button @click="editTask"><i class="fa-regular fa-pen-to-square"></i></button>
            <button @click="deleteTask"><i class="fa-regular fa-trash-can"></i></button>
        </div>
    </div>
</template>

<script>
export default {
    name: "TodoItem",
    props: {
        todo: Object
    },
    methods: {
        toggleStatus() {
            this.$emit('toggleStatus', this.todo.id)
        },
        editTask() {
            this.$emit('editTask', this.todo.id)
        },
        deleteTask() {
            this.$emit('delete', this.todo.id)
        }
    }
}
</script>


<style scoped>
    .todo-item {
        height: 100%;
        width: 100%;
        padding: 0.3rem 0.5rem;
        border: 1px solid #dacac1ee;
        border-radius: 0.4rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 1rem;
    }
    .left-side, .right-side {
        color: #dacac1d8;
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }
    .left-side #task-status {
        height: 1.6rem;
        width: 1.6rem;
        border-radius: 50%;
        border: 2px solid orangered;
        cursor: pointer;
    }
    .left-side span.taskcompleted {
        border: none !important;
        background-color: rgb(68, 240, 68);
    }
    .left-side input[type="text"] {
        height: 1.4rem;
        border: none;
        color: #dacac1d8;
        border-bottom: 1px solid #dacac17a;
        background-color: transparent;
    }
    .left-side input[type="text"]:focus {
        outline: none;
        background-color: transparent;
    }

    .right-side button {
        color: #dacac1d8;
        height: 2.5rem;
        width: 2.5rem;
        font-size: 1.4rem;
        border: none;
        border-radius: 50%;
        background-color: transparent;
        cursor: pointer;
    }
    
    .completed {
        text-decoration: line-through;
    }
</style>