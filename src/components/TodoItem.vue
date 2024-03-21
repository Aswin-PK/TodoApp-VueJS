<template>
    <div class="todo-item">
        <div class="left-side">
            <span @click="toggleStatus" id="task-status" :class="{ taskcompleted: todo.isCompleted }"></span>
            <span :class="{ completed: todo.isCompleted }">{{ todo.task }}</span>
            <span :class="todo.priority">{{ todo.priority }}</span>
        </div>
        <div class="right-side">
            <button @click="editTask"><i class="fa-regular fa-pen-to-square"></i></button>
            <button @click="deleteTask"><i class="fa-regular fa-trash-can"></i></button>
            <div class="swap-arrows">
                <span @click="handleSwap('up')"><i class="fa-solid fa-sort-up"></i></span>
                <span @click="handleSwap('down')"><i class="fa-solid fa-sort-down"></i></span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TodoItem",
    props: {
        todo: Object,
        index: Number
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
        },
        handleSwap(direction) {
            this.$emit('swap', direction, this.index)
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

    .swap-arrows {
        height: 2rem;
        display: flex;
        align-items: center;
        /* justify-content: space-between; */
        flex-direction: column;
        gap:0.2em;
    }
    .swap-arrows span {
        height: 1em;
        display: grid;
        place-content: center;
        cursor: pointer;
    }
    .swap-arrows span i {
        line-height: 1em;
        font-size: 1.2em;
        color: #a8a8a889;
    }
    .swap-arrows span:active i{
        color: #dbdbdbd0;
    }


    .left-side .low, .medium, .high {
        font-size: 0.9em;
        width: auto;
        color: #333;
        padding: 0 0.3em;
    }
    .left-side .low {
        background-color: rgb(255, 245, 112);
    }
    .left-side .medium {
        background-color: rgb(255, 170, 79);
    }
    .left-side .high {
        background-color: rgb(255, 92, 63);
    }
</style>