<template>
    <div class="form-group">
        <label>{{ label }}</label>
        <input @input="onInput" :value="subject" type="text" class="form-control mt-2 mb-2">
        <div v-if="error" class="text-red">{{ error }}</div>
    </div>
</template>

<script>
export default {
    props: {
        label: {
            type: String,
            required: true
        },
        error: {
            type: String,
            required: true
        },
        subject: {
            type: String,
            required: true
        }
    },
    setup(props, { emit }) {
        const onInput = (e) => {
            emit('update:subject', e.target.value);
        }

        return {
            onInput
        }
    }

}
</script>

<style>
.text-red {
    color: red
}
</style>