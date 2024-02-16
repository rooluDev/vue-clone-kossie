<template>
    <div v-if="loading">Loading...</div>
    <form v-else @submit.prevent="onSave">
        <div class="row">
            <!-- <div class="col-6">
                <div class="form-group">
                    <label>Subject</label>
                    <input v-model="todo.subject" type="text" class="form-control mt-2 mb-2">
                    <div v-if="subjectError" class="text-red">{{ subjectError }}</div>
                </div>
            </div> -->
            <Input label="Subject" v-model:subject="todo.subject" :error="subjectError" />
            <div v-if="editing" class="col-6">
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

            <div class="col-12">
                <div class="form-group">
                    <label>Body</label>
                    <textarea v-model="todo.body" class="form-control" cols="30" rows="10"></textarea>
                </div>
            </div>

        </div>
        <button :disabled="!todoUpdated" type="submit" class="btn btn-primary">{{ editing ? 'Update' : 'create' }}</button>
        <button class="btn btn-outline-dark m-2" @click="moveToTodoListPage">Cancle</button>
    </form>
    <Transition name="fade">
        <Toast v-if="showToast" :message="toastMessage" :type="toastAlertType" />
    </Transition>
</template>

<script>
import { useRoute, useRouter } from 'vue-router';
import axios from '@/axios';
import { computed, onUpdated, ref } from 'vue';
import _ from 'lodash';
import Toast from '@/components/Toast.vue';
import { useToast } from '@/composables/toast';
import Input from '@/components/Input.vue';

export default {
    components: {
        Toast,
        Input
    },
    props: {
        editing: {
            type: Boolean,
            default: false
        }
    },
    setup(props) {
        const route = useRoute();
        const router = useRouter();
        const todo = ref({
            subject: '',
            completed: false,
            body: ''
        });

        const subjectError = ref('');
        const originalTodo = ref(null);
        const loading = ref(false);
        const todoId = route.params.id;
        const { showToast, toastAlertType, toastMessage, triggerToast } = useToast();

        const getTodo = async () => {
            loading.value = true;
            try {
                const res = await axios.get(`todos/${todoId}`);
                todo.value = { ...res.data };
                originalTodo.value = { ...res.data };
                loading.value = false;
            } catch (error) {
                loading.value = false;
                console.log(error);
                triggerToast('Something went wrong', 'danger');
            }
        }

        const todoUpdated = computed(() => {
            return !_.isEqual(todo.value, originalTodo.value);
        })

        const toggleTodoStatus = () => {
            todo.value.completed = !todo.value.completed;
        }

        const moveToTodoListPage = () => {
            router.push({
                name: 'Todos'
            })
        }

        if (props.editing) {
            getTodo();
        }

        const onSave = async () => {
            subjectError.value = '';
            if (!todo.value.subject) {
                subjectError.value = 'Subject is required';
                return;
            }
            try {
                let res;
                const data = {
                    subject: todo.value.subject,
                    completed: todo.value.completed,
                    body: todo.value.body
                }
                if (props.editing) {
                    res = await axios.put(`todos/${todoId}`, data);
                    originalTodo.value = { ...res.data };
                } else {
                    res = await axios.post(`todos`, data);
                    todo.value.subject = '';
                    todo.value.body = '';
                }
                const message = 'Successfully ' + (props.editing ? 'Updated' : 'Created');
                triggerToast(message);
            } catch (error) {
                console.log(error);
                triggerToast('Something went wrong', 'danger');
            }
        }

        return {
            todo,
            loading,
            toggleTodoStatus,
            moveToTodoListPage,
            onSave,
            todoUpdated,
            showToast,
            toastMessage,
            toastAlertType,
            subjectError,
        };
    }
}

</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
    transform: translateY(-30px);
}

.fade-enter-to,
.fade-leave-from {
    opacity: 0;
    transform: translateY(0px);
}
</style>