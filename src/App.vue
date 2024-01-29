<template>
    <div :class="nameClass">{{ name }}</div>
    <input :type="type" :value="name" />
    <button class="btn btn-primary" @click="updateName">click</button>
</template>

<script>
import { ref } from "vue";

export default {
    setup() {
        const name = ref("roolu");
        const type = ref("number");
        const nameClass = ref("");

        const updateName = () => {
            name.value = "Dev";
            type.value = "text";
            nameClass.value = "name";
        };

        return {
            name,
            updateName,
            type,
            nameClass,
        };
    },
};
</script>

<style>
.name {
    color: red;
}
</style>
