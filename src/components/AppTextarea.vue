<template>
    <div class="form-control">
        <label :for="id"> {{ label }} </label>
        <textarea
            :rows="rows"
            :id="id"
            :placeholder="placeholder"
            :value="modelValue"
            @input="change"
        ></textarea>
    </div>
</template>

<script>
export default {
    props: {
        rows: {
            type: String,
            required: false,
            default: '3',
        },
        id: {
            type: String,
            required: true,
        },
        label: {
            type: String,
            required: true,
        },
        placeholder: {
            type: String,
            required: false,
        },
        modelValue: {
            type: String,
            required: false,
        },
    },
    emits: ['update:modelValue'],
    methods: {
        change(evt) {
            this.$emit('update:modelValue', evt.target.value);
        },
    },
}
</script>
