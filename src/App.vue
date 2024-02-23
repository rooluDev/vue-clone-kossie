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
    <Toast />
</template>

<script>
import Toast from '@/components/Toast.vue';

export default {
    components: {
        Toast
    },
}
</script>

<style scoped></style>
