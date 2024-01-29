<template>
    <div class="name">{{ name }}</div>
    <button class="btn btn-primary" @click="updateName">click</button>
</template>

<script>
export default {
    setup() {
        let name = "roolu Dev";

        // const greeting = (name) => {
        //   return "Hello " + name;
        // };

        // const greet = greeting(name);

        const updateName = () => {
            name = "roolu";
        };

        return {
            name,
            updateName,
        };
    },
};
</script>

<style>
.name {
    color: red;
}
</style>
