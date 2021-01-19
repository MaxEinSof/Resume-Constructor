<template>
    <li class="list-item">
        <div>
            <p><strong> {{ email }} </strong></p>
            <small> {{ body }} </small>
        </div>
    </li>
</template>

<script>
export default {
    props: {
        email: {
            type: String,
            required: true,
            validator(str) {
                const re = /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/;
                return re.test(str.toLowerCase());
            }
        },
        body: {
            type: String,
            required: true,
        },
    },
}
</script>
