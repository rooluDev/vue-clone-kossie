<template>
    <div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">
        <div class="card-body p-2 d-flex align-items-center" style="cursor: pointer" @click="moveToPage(todo.id)">
            <div class="flex-grow-1">
                <input class="ms-2 me-2" type="checkbox" :checked="todo.completed" @change="toggleTodo(index, $event)"
                    @click.stop />
                <span :class="{ todo: todo.completed }">{{ todo.subject }}</span>
            </div>
            <div>
                <button class="btn btn-danger btn-sm" @click.stop="openModal(todo.id)">delete</button>
            </div>
        </div>
    </div>
    <Modal v-if="showModal" @close="closeModal" />
</template>

<script>
import { useRouter } from 'vue-router';
import Modal from '@/components/Modal.vue';
import { ref } from 'vue';
export default {
    components: {
        Modal
    },
    props: {
        todos: {
            type: Array,
            required: true,
        },
    },

    emits: ["toggle-todo", "delete-todo"],

    setup(props, { emit }) {
        const router = useRouter();
        const showModal = ref(false);
        const todoDeleted = ref(null);

        const openModal = (id) => {
            todoDeleted.value = id;
            showModal.value = true;
        }

        const closeModal = () => {
            todoDeleted.value = null;
            showModal.value = false;
        }

        const toggleTodo = (index, event) => {
            emit("toggle-todo", index, event.target.checked);
        };

        const deleteTodo = (index) => {
            emit("delete-todo", index);
        };

        const moveToPage = (todoId) => {
            // router.push('/todos/' + todoId);
            router.push({
                name: 'Todo',
                params: {
                    id: todoId
                }
            })
        }

        return {
            toggleTodo,
            deleteTodo,
            moveToPage,
            showModal,
            openModal,
            closeModal,
        };
    },
};
</script>

<style></style>
