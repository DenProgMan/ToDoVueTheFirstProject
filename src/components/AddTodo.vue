<template>
    <form @submit.prevent="onSubmit">
        <input type="text" v-model="title">
        <button type="submit">Create</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            title: '',
        }
    },
    methods: {
        onSubmit() {
            if (this.title.trim()) {
                const newTodo = {
                    id: Date.now(),
                    title: this.title,
                    completed: false,
                    userId: 1,
                }
                this.$emit('add-todo', newTodo);
                this.title = '';
            }
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
        justify-content: center; 
    }
    input {
        width: 400px;
        margin-right: 15px;
    }
    /* button {
        margin-left: auto;
    } */
</style>