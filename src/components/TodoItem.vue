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
            <button class="swapper" @click="handleSwap(todo, index)">
                <i class="fa-solid fa-sort" :class="todo.isSwappable ? 'swap-on' : ''"></i>
            </button>
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
    data() {
        return {
            swapIndex1: null,
            swapIndex2: null
        }
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
        handleSwap(todo, index) {
            todo.isSwappable = !todo.isSwappable
            this.$emit('swap', index)
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

    .swapper .swap-on {
        color: green
    }
</style>