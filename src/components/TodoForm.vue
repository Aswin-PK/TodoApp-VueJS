<template>
    <form @submit.prevent="handleSubmit">
        <div class="priority-selectors">
            <span>Select Priority:</span>
            <span>
                <input type="radio" v-model="selectedPriority" name="priority" id="low" value="low" />
                <label for="low">Low</label>
            </span>
            <span>
                <input type="radio" v-model="selectedPriority" name="priority" id="medium" value="medium" />
                <label for="medium">Medium</label>
            </span>
            <span>
                <input type="radio" v-model="selectedPriority" name="priority" id="high" value="high" />
                <label for="high">High</label>
            </span>
        </div>
        <div class="task-input">
            <input type="text" v-model="taskInput" id="new-task" placeholder="Write your next task" />
            <button type="submit">+</button>
        </div>
    </form>
</template>


<script>
export default {
    name: "TodoForm",

    props: {
        initialValue: String
    },

    data() {
        return {
            taskInput: '',
            selectedPriority: "medium"
        };
    },

    watch: {
        initialValue: {
            handler(newVal) {
                // Update taskInput whenever taskToBeEdited changes
                this.taskInput = newVal || '';
            },
            immediate: true // Trigger the handler immediately on component creation
        }
    },

    methods: {
        handleSubmit() {
            console.log('this.selected', this.selectedPriority)
            if (this.taskInput.trim() === '') return;  // handle case when nothing entered in the todo input box

            // if (this.taskToBeEdited) {
            //     console.log('todoId', this.taskToBeEdited.id, 'editedTask:', this.taskInput)
            //     this.$emit('edit', { todoId: this.taskToBeEdited.id, editedTask: this.taskInput })
            // }

            else {
                // console.log('this.taskInput', this.taskInput)
                this.$emit('handleInput', this.taskInput, this.selectedPriority);
            }
            // Reset the input field
            this.taskInput = '';
        }
    }

}
</script>

<style scoped>
form {
    width: 100%;
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 1rem;
    margin: 0.7rem 0;
}

.task-input {
    width: 100%;
    display: flex;
    gap: 0.5rem;
}

input[type="text"] {
    height: 2.5rem;
    width: 100%;
    border-radius: 1em;
    border: none;
    padding: 0 0.7rem;
    color: rgba(216, 203, 184, 0.941);
    outline: none;
    background-color: #eee1cb1d
}

button {
    height: 2.5rem;
    font-size: 1.6em;
    background-color: orangered;
    border: none;
    border-radius: 50%;
    aspect-ratio: 1;
    cursor: pointer;
}

/* Prority Selector styles */
.priority-selectors {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    color: white;
}

.priority-selectors span {
    display: flex;
    gap: 0.5em;
}

.priority-selectors span input[type="radio"], label {
    cursor: pointer;
}
</style>