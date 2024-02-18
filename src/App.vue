<template>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <router-link class="navbar-brand" :to="{ name: 'Home' }">Roolu Dev</router-link>

            <ul class="navbar-nav me-auto mb-lg-0">
                <li class="nav-item">
                    <router-link class="nav-link active" aria-current="page" :to="{ name: 'Todos' }">Todos</router-link>
                </li>
            </ul>
        </div>
    </nav>
    <div class="container">
        <router-view />
    </div>
    <transition name="slide">
        <Toast v-if="showToast" :message="toastMessage" :type="toastAlertType" />
    </transition>
</template>

<script>
import Toast from '@/components/Toast.vue';
import { useToast } from './composables/toast';

export default {
    components: {
        Toast
    },
    setup() {
        const { showToast, toastAlertType, toastMessage, triggerToast } = useToast();

        return {
            showToast,
            toastAlertType,
            toastMessage,
            triggerToast
        }
    }
}
</script>

<style scoped>
.slide-enter-active,
.slide-leave-active {
    transition: opacity 0.5s ease;
}

.slide-enter-from,
.slide-leave-to {
    opacity: 0;
    transform: translateY(-30px);
}

.slide-enter-to,
.slide-leave-from {
    opacity: 0;
    transform: translateY(0px);
}
</style>
