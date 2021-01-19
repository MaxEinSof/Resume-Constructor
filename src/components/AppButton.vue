<template>
    <button
        class="btn primary"
        :type="type"
        :disabled="isDisabled"
        @click="$emit('action')"
    >
        <slot>Кнопка</slot>
    </button>
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            required: false,
            default: 'button',
        },
        isDisabled: {
            type: Boolean,
            required: false,
            default: false,
        },
    },
    emits: ['action'],
}
</script>
