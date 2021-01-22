<template>
    <div :class="['alert', alert.type]">
        <h3> {{ alert.title }} </h3>
        <p> {{ alert.text }} </p>
    </div>
</template>

<script>
export default {
    mounted() {
        setTimeout(() => {
            this.$emit('hide-alert');
        }, 4000);
    },
    props: {
        alert: {
            type: Object,
            required: true,
            validator(obj) {
                return 'type' in obj && 'title' in obj && 'text' in obj;
            }
        },
    },
    emits: ['hide-alert'],
}
</script>
