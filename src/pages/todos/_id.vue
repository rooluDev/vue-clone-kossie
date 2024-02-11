<template>
    <h1>To-Do Page</h1>
    <div v-if="loading">Loading...</div>
    <form v-else>
        <div class="row">
            <div class="col-6">
                <div class="form-group">
                    <label>Subject</label>
                    <input v-model="todo.subject" type="text" class="form-control mt-2 mb-2">
                </div>
            </div>
            <div class="col-6">
                <div class="form-group">
                    <label>Status</label>
                    <div>
                        <button type="button" class="btn" :class="todo.completed ? 'btn-success' : 'btn-danger'"
                            @click="toggleTodoStatus">
                            {{ todo.completed ? 'Completed' : 'Not Completed' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <button type="submit" class="btn btn-primary">save</button>
        <button class="btn btn-outline-dark m-2" @click="moveToTodoListPage">Cancle</button>
    </form>
</template>

<script>
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
import { ref } from 'vue';

export default {
    setup() {
        const route = useRoute();
        const router = useRouter();
        const todo = ref(null);
        const loading = ref(true);

        const getTodo = async () => {
            const res = await axios.get('http://localhost:3000/todos/' + route.params.id);
            todo.value = res.data;
            loading.value = false;
        }

        const toggleTodoStatus = () => {
            todo.value.completed = !todo.value.completed;
        }

        const moveToTodoListPage = () => {
            router.push({
                name: 'Todos'
            })
        }

        getTodo();


        return {
            todo,
            loading,
            toggleTodoStatus,
            moveToTodoListPage,
        };
    }
}

</script>

<style></style>