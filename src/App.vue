<template>
    <div class="container">
        <h2 class="mt-2">To-Do List</h2>
        <input class="form-control" type="text" v-model="searchText" placeholder="Search" />
        <hr />
        <TodoSimpleForm @add-todo="addTodo" />
        <div v-if="!filtreredTodos.length">There is nothig to display</div>
        <TodoList :todos="filtreredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
    </div>
</template>

<script>
import { ref, computed } from "vue";
import TodoSimpleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";

export default {
    components: {
        TodoSimpleForm,
        TodoList,
    },

    setup() {
        const todos = ref([]);
        const todoStyle = {
            textDecoration: "line-through",
            color: "gray",
        };

        const addTodo = (todo) => {
            todos.value.push(todo);
        };

        const toggleTodo = (index) => {
            todos.value[index].completed = !todos.value[index].completed;
        };

        const deleteTodo = (index) => {
            todos.value.splice(index, 1);
        };

        const searchText = ref("");

        const filtreredTodos = computed(() => {
            if (searchText.value) {
                return todos.value.filter((todo) => {
                    return todo.subject.includes(searchText.value);
                });
            }

            return todos.value;
        });

        return {
            todos,
            todoStyle,
            searchText,
            addTodo,
            deleteTodo,
            toggleTodo,
            filtreredTodos,
        };
    },
};
</script>

<style>
.todo {
    color: gray;
    text-decoration: line-through;
}
</style>
