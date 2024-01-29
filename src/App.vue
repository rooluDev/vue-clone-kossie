<template>
  <div class="name">{{ name }}</div>
</template>

<script>
export default {
  setup() {
    const name = "roolu Dev1";

    return {
      name,
    };
  },
};
</script>

<style>
.name {
  color: red;
}
</style>
