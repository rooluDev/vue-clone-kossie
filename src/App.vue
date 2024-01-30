<template>
    <!-- <div v-show="toggle">true</div>
    <div v-show="!toggle">false</div>
    <div v-if="toggle">true</div>
    <div v-else>false</div>
    <button @click="onToggle">Toggle</button> -->
    <div class="container">
        <h2 class="mt-2">To-Do List</h2>
        <form @submit.prevent="onSubmit">
            <div class="d-flex">
                <div class="flex-grow-1 me-2">
                    <input class="form-control" type="text" v-model="todo" placeholder="Type new to-do" />
                </div>
                <div>
                    <button class="btn btn-primary" type="submit">Add</button>
                </div>
            </div>

            <div v-show="hasError" style="color: red">This field cannot be empty</div>
        </form>
        <div v-for="todo in todos" :key="todo.id" class="card mt-2">
            <div class="card-body p-2">
                {{ todo.subject }}
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from "vue";

export default {
    setup() {
        // const toggle = ref(false);
        const todo = ref("");
        const todos = ref([
            { id: 1, subject: "buy apple" },
            { id: 2, subject: "buy banana" },
        ]);

        const hasError = ref(false);

        const onSubmit = () => {
            if (todo.value === "") {
                hasError.value = true;
            } else {
                hasError.value = false;
                todos.value.push({
                    id: Date.now(),
                    subject: todo.value,
                });
            }
        };

        // const onToggle = () => {
        //     toggle.value = !toggle.value;
        // };

        return {
            todo,
            todos,
            // toggle,
            hasError,
            onSubmit,
        };
    },
};
</script>

<style>
.name {
    color: red;
}
</style>
