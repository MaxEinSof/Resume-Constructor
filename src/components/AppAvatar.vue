<template>
    <div class="avatar">
        <img :src="avatarUrl">
    </div>
</template>

<script>
export default {
    props: {
        avatarUrl: {
            type: String,
            required: true,
            validator(str) {
                return str.startsWith('http');
            }
        },
    },
}
</script>
