<template>
    <form @submit.prevent="addTodo">
        <input type="text" v-model="newTask" id="new-task" placeholder="Write your next task" />
        <button type="submit">+</button>
    </form>
</template>


<script>
export default {
    name: "TodoForm",
    data() {
        return {
            newTask: ''
        };
    },
    methods: {
        addTodo() {
            if(this.newTask.trim() === '') return  // handle case when nothing entered in the todo input box
            this.$emit('add', this.newTask);
            this.newTask = ''
        }
    }

}
</script>

<style scoped>
    form {
        width: 100%;
        display: flex;
        align-items: center;
        gap: 1rem;
        margin: 0.7rem 0;
    }
    input {
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
</style>