<template>
    <div>
        <h2>Todo application</h2>
        <router-link to="/">Home</router-link>
        <AddTodo
            @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not completed</option>
        </select>
        <hr>
        <Loader v-if="loading" />
        <TodoList
            v-else-if="filteredTodos.length"
            v-bind:todos="filteredTodos"
            @remove-todo="removeTodo"
        />
        <p v-else>No todo</p>
    </div>
</template>

<script>

import TodoList from '@/components/TodoList';
import AddTodo from '@/components/AddTodo';
import Loader from '@/components/Loader';

export default {
    name: 'App',
    data() {
        return {
            todos: [],
            loading: true,
            filter: 'all',
        }
    },
    async mounted() {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=10');
        const result = await response.json();

        setTimeout(() => {
            this.todos = result;
            this.loading = false;
        }, 1000);
    },
    watch: {
        filter(value) {
            console.log(value); //FIXME: Удалить этот console log
        }
    },
    computed: {
        filteredTodos() {
            switch (this.filter) {
                case 'completed':
                    return this.todos.filter(t => t.completed);
                    break;
                case 'not-completed':
                    return this.todos.filter(t => !t.completed);
                    break;
            
                default:
                    return this.todos;
                    break;
            }
        }
    },
    methods: {
        removeTodo(id) {
            this.todos = this.todos.filter(t => t.id !== id);
        },
        addTodo(todo) {
            this.todos.push(todo);
        }
    },
    components: {
        TodoList, AddTodo, Loader
    }
}
</script>